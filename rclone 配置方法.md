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
nano rcloned
```

添加代码https://github.com/twtmactt/test/blob/master/rcloned%E6%8C%82%E8%BD%BD%E5%BC%80%E6%9C%BA%E8%87%AA%E5%90%AF%E8%84%9A%E6%9C%AC  

修改以下内容：  
NAME=""  #rclone name名，及配置时输入的Name  
REMOTE=''  #远程文件夹，Google Drive网盘里的挂载的一个文件夹  
LOCAL=''  #挂载地址，VPS本地挂载目录  

2.设定自启  
```
mv rcloned /etc/init.d/rcloned
chmod +x /etc/init.d/rcloned
nano /ect/rc.d/rc.local
在末尾加入一行 bash /ect/init.d/rcloned start
chmod +x /ect/rc.d/rc.local
bash /ect/init.d/rcloned status
```

