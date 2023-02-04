1.搭建aria2 
```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/aria2.sh && chmod +x aria2.sh && bash aria2.sh 
```

2.安装rclone  
```sudo -v ; curl https://rclone.org/install.sh | sudo bash```  
官方地址：https://rclone.org/downloads/


3.客户端授权  
在本地Windows电脑上下载rclone，下载地址：https://rclone.org/downloads/。   
然后解压出来，比如我解压到D盘，文件夹命名rclone，此时点击Win+R，然后输入cmd，确定。再输入以下命令：
```
cd /d d:\rclone
rclone authorize "onedrive" "Client_ID" "Client_secret"
```
复制token:  
{"access_token":"xxxx"}  
<---End paste    #请复制{xx}整个内容，后面需要用到

4.配置rclone，添加网盘  

5.配置上传脚本  
```nano autoupload.sh```  
复制粘贴修改以下内容 

```
#!/bin/bash

filepath=$3	 #取文件原始路径，如果是单文件则为/Download/a.mp4，如果是文件夹则该值为文件夹内第一个文件比如/Download/a/1.mp4
path=${3%/*}	 #取文件根路径，如把/Download/a/1.mp4变成/Download/a
downloadpath='/data/Download'	#Aria2下载目录
name='codesofun' #配置Rclone时的name
folder='/share'	 #网盘里的文件夹，如果是根目录直接留空
MinSize='10k'	 #限制最低上传大小，默认10k，BT下载时可防止上传其他无用文件。会删除文件，谨慎设置。
MaxSize='15G'	 #限制最高文件大小，默认15G，OneDrive上传限制。

if [ $2 -eq 0 ]; then exit 0; fi

while true; do
if [ "$path" = "$downloadpath" ] && [ $2 -eq 1 ]	#如果下载的是单个文件
    then
    rclone move -v "$filepath" ${name}:${folder} --min-size $MinSize --max-size $MaxSize
    rm -vf "$filepath".aria2	#删除残留的.aria.2文件
    exit 0
elif [ "$path" != "$downloadpath" ]	#如果下载的是文件夹
    then
    while [[ "`ls -A "$path/"`" != "" ]]; do
    rclone move -v "$path" ${name}:/${folder}/"${path##*/}" --min-size $MinSize --max-size $MaxSize --delete-empty-src-dirs
    rclone delete -v "$path" --max-size $MinSize	#删除多余的文件
    rclone rmdirs -v "$downloadpath" --leave-root	#删除空目录，--delete-empty-src-dirs 参数已实现，加上无所谓。
    done
    rm -vf "$path".aria2	#删除残留的.aria2文件
    exit 0
fi
done
```

保存后给予执行权限  
```chmod +x autoupload.sh```


编辑aria2的配置文件   
```
nano /root/.aria2/aria2.conf  
on-download-complete=/root/autoupload.sh  
/etc/init.d/aria2 restart
```  


每月自动删除存留资源参考  
创建文件auto-del-aria2-download.sh  
内容：（downloads是下载目录）  
#!/bin/sh
find downloads -type f | xargs rm -rf  
crontab -e定时执行  
0 2 1 * * auto-del-aria2-download.sh > /dev/null 2>&1




