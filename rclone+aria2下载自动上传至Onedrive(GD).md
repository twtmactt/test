1.搭建aria2 
```
apt install sudo wget curl ca-certificates  
wget -N git.io/aria2.sh && chmod +x aria2.sh  
./aria2.sh  
```

2.安装rclone  
```sudo -v ; curl https://rclone.org/install.sh | sudo bash```  
官方地址：https://rclone.org/downloads/


3.客户端授权（上传GD跳过这一步）  
在本地Windows电脑上下载rclone，下载地址：https://rclone.org/downloads/。   
然后解压出来，比如我解压到D盘，文件夹命名rclone，此时点击Win+R，然后输入cmd，确定。再输入以下命令：
```
cd /d d:\rclone
rclone authorize "onedrive"
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
#=================================================
# Description: Aria2 download completes calling Rclone upload
# Lisence: MIT
# Version: 1.8
# Author: P3TERX
# Blog: https://p3terx.com
#=================================================
 
downloadpath='/usr/local/caddy/www/file' #Aria2下载目录
name='remote' #配置Rclone时填写的name
folder='/backup' #网盘里的文件夹，留空为整个网盘。
retry_num=3 #上传失败重试次数
 
#=================下面不需要修改===================
filepath=$3 #Aria2传递给脚本的文件路径。BT下载有多个文件时该值为文件夹内第一个文件，如/root/Download/a/b/1.mp4
rdp=${filepath#${downloadpath}/} #路径转换，去掉开头的下载路径。
path=${downloadpath}/${rdp%%/*} #路径转换。下载文件夹时为顶层文件夹路径，普通单文件下载时与文件路径相同。
 
Task_INFO(){
echo
echo -e "[\033[1;32mUPLOAD\033[0m] Task information:"
echo -e "————————– [\033[1;33mINFO\033[0m] ————————–"
echo -e "\033[1;35mDownload path：\033[0m${downloadpath}"
echo -e "\033[1;35mFile path: \033[0m${filepath}"
echo -e "\033[1;35mUpload path: \033[0m${uploadpath}"
echo -e "\033[1;35mRemote path：\033[0m${remotepath}"
echo -e "————————– [\033[1;33mINFO\033[0m] ————————–"
echo
}
 
Upload(){
retry=0
while [ $retry -le $retry_num -a -e "${uploadpath}" ]; do
[ $retry != 0 ] && echo && echo -e "Upload failed! Retry ${retry}/${retry_num} …" && echo
rclone move -v "${uploadpath}" "${remotepath}"
rclone rmdirs -v "${downloadpath}" –leave-root
retry=$(($retry+1))
done
[ -e "${uploadpath}" ] && echo && echo -e "Upload failed: ${uploadpath}" && echo
[ -e "${path}".aria2 ] && rm -vf "${path}".aria2
[ -e "${filepath}".aria2 ] && rm -vf "${filepath}".aria2
}
 
if [ $2 -eq 0 ]
then
exit 0
fi
 
echo && echo -e " \033[1;33mU P L O A D ! ! !\033[0m" && echo
echo && echo -e " \033[1;32mU P L O A D ! ! !\033[0m" && echo
echo && echo -e " \033[1;35mU P L O A D ! ! !\033[0m" && echo
 
if [ "$path" = "$filepath" ] && [ $2 -eq 1 ] #普通单文件下载，移动文件到设定的网盘文件夹。
then
uploadpath=${filepath}
remotepath="${name}:${folder}"
Task_INFO
Upload
exit 0
elif [ "$path" != "$filepath" ] && [ $2 -gt 1 ] #BT下载（文件夹内文件数大于1），移动整个文件夹到设定的网盘文件夹。
then
uploadpath=${path}
remotepath="${name}:${folder}/${rdp%%/*}"
Task_INFO
Upload
exit 0
elif [ "$path" != "$filepath" ] && [ $2 -eq 1 ] #第三方度盘工具下载（子文件夹或多级目录等情况下的单文件下载）、BT下载（文件夹内文件数等于1），移动文件到设定的网盘文件夹下的相同路径文件夹。
then
uploadpath=${filepath}
remotepath="${name}:${folder}/${rdp%/*}"
Task_INFO
Upload
exit 0
fi
Task_INFO

```

保存后给予执行权限  
```chmod +x autoupload.sh```


编辑aria2的配置文件   
```
nano /root/.aria2c/aria2.conf  
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




