软件地址https://github.com/rclone/rclone/releases  
Debian / Ubuntu 安装：（如果安装特定版本，地址和解压要改）  
```
wget https://downloads.rclone.org/rclone-current-linux-amd64.zip  

unzip rclone-current-linux-amd64.zip  

chmod 0777 ./rclone-*/rclone  

cp ./rclone-*/rclone /usr/bin/  

rm -rf ./rclone-*  
```

适用于ARM框架安装命令：（同上）  
```
wget https://downloads.rclone.org/rclone-current-linux-arm64.zip  

unzip rclone-current-linux-arm64.zip  

chmod 0777 ./rclone-*/rclone  

cp ./rclone-*/rclone /usr/bin/  

rm -rf ./rclone-*  
```

卸载rclone  
```
sudo rm /usr/bin/rclone
sudo rm /usr/local/share/man/man1/rclone.1
```
