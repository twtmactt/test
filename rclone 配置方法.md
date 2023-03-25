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

