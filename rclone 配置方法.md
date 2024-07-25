1、安装rclone
```
sudo -v ; curl https://rclone.org/install.sh | sudo bash
```

2、初始化配置
```
rclone config
```

3、挂载为磁盘  
#新建本地文件夹，路径自己定，即下面的LocalFolder  
```
mkdir /root/GoogleDrive
```

#挂载为磁盘  
```
rclone mount codesofun:share /data/GoogleDrive --allow-other --allow-non-empty --vfs-cache-mode writes &
或者
rclone mount DriveName:Folder LocalFolder --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
webdav挂载示例
rclone mount codesofun:/share /data/GoogleDrive --allow-other --allow-non-empty --vfs-cache-mode writes &
```

DriveName为初始化配置填的name，Folder为Google Drive里的文件夹，LocalFolder为VPS上的本地文件夹。  

4、卸载磁盘  
```
fusermount -qzu LocalFolder
```

自启动  
1、添加并编辑脚本  
```
command="mount codesofun:share /data/GoogleDrive --allow-other --allow-non-empty --vfs-cache-mode writes &"    

cat > /etc/systemd/system/rclone.service <<EOF
[Unit]
Description=Rclone
After=network-online.target
 
[Service]
Type=simple
ExecStart=$(command -v rclone) ${command}
Restart=on-abort
User=root
 
[Install]
WantedBy=default.target
EOF

systemctl start rclone  
systemctl enable rclone
```
### Rclone复制/移动文件参考命令  
```
rclone copy -v  ode5:/movies/Movies 115:/115/Movies --transfers=3  --progress
```
其中 "ode5:/" "115:/115"的这种格式是我只用rclone添加了储存，并未挂载到vps上，如果已挂载，可能会有所区别，[参考文章](https://p3terx.com/archives/rclone-advanced-user-manual-common-command-parameters.html)
