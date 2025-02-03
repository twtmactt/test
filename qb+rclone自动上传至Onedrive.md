### 1. 配置rclone [配置方法](https://github.com/twtmactt/test/blob/master/rclone%20%E9%85%8D%E7%BD%AE%E6%96%B9%E6%B3%95.md)  

### 2. 安装qBittorrent  
qBittorrent下载地址[点此直达](https://github.com/userdocs/qbittorrent-nox-static/releases)  
- 安装qBittorrent
```
wget -qO /usr/local/bin/x86_64-qbittorrent-nox 下载地址  
chmod 700 /usr/local/bin/x86_64-qbittorrent-nox
```  

- 编辑文件  
```
nano /etc/systemd/system/qbt.service
```  
添加如下内容  
```
[Unit]
Description=qBittorrent Service
After=network.target nss-lookup.target

[Service]
UMask=000
ExecStart=/usr/local/bin/x86_64-qbittorrent-nox --profile=/usr/local/etc

[Install]

WantedBy=multi-user.target
```  
注：--profile=/usr/local/etc这个选项，表示qBittorrent的配置目录，如果不写，就是在当前用户的主目录，例如root目录下面。  

- 基本操作  
```
systemctl enable qbt   #开机启动，第一次必须要执行此命令
systemctl start qbt  #启动
systemctl stop qbt  #停止
systemctl status qbt  #软件运行状态查询 
```  

qBittorrent就算安装成功了，打开浏览器，输入：IP:8080，就可以打开qBittorrent的网页界面了  
用户名：admin  
密码：adminadmin  

### 3. 配置自动上传  
root目录下新建上传脚本  
```
nano qb_auto.sh  
```
粘贴如下内容，并按照说明修改  
```
#!/bin/bash
torrent_name=$1
content_dir=$2
root_dir=$3
save_dir=$4
files_num=$5
torrent_size=$6
file_hash=$7
qb_version="4.3.6"    # 改成你的qbit版本
qb_username="admin"    # qbit用户名
qb_password="adminadmin"    # qbit密码
qb_web_url=""    # qbit webui地址
leeching_mode="true"    # 吸血模式，true下载完成后自动删除本地种子和文件
log_dir="/root/qbauto"    # 日志输出目录
rclone_dest=""    # rclone配置的储存名
rclone_parallel="16"    # qbit上传线程，默认4
auto_del_flag="rclone"    # 添加标签或者分类来标识已上传的种子 v4.0.4+版本添加标签“rclone”，低版本通过添加分类“rclone”标识
# 改上面的那些参数即可
 
if [ ! -d ${log_dir} ]
then
        mkdir -p ${log_dir}
fi
 
version=$(echo $qb_version | grep -P -o "([0-9]\.){2}[0-9]" | sed s/\\.//g)
 
function qb_login(){
        if [ ${version} -gt 404 ]
        then
                qb_v="1"
                cookie=$(curl -i --header "Referer: ${qb_web_url}" --data "username=${qb_username}&password=${qb_password}" "${qb_web_url}/api/v2/auth/login" | grep -P -o 'SID=\S{32}')
                if [ -n ${cookie} ]
                then
                        echo "[$(date '+%Y-%m-%d %H:%M:%S')] 登录成功！cookie:${cookie}" >> ${log_dir}/autodel.log
 
                else
                        echo "[$(date '+%Y-%m-%d %H:%M:%S')] 登录失败！" >> ${log_dir}/autodel.log
                fi
        elif [[ ${version} -le 404 && ${version} -ge 320 ]]
        then
                qb_v="2"
                cookie=$(curl -i --header "Referer: ${qb_web_url}" --data "username=${qb_username}&password=${qb_password}" "${qb_web_url}/login" | grep -P -o 'SID=\S{32}')
                if [ -n ${cookie} ]
                then
                        echo "[$(date '+%Y-%m-%d %H:%M:%S')] 登录成功！cookie:${cookie}" >> ${log_dir}/autodel.log
                else
                        echo "[$(date '+%Y-%m-%d %H:%M:%S')] 登录失败" >> ${log_dir}/autodel.log
                fi
        elif [[ ${version} -ge 310 && ${version} -lt 320 ]]
        then
                qb_v="3"
                echo "陈年老版本，请及时升级"
                exit
        else
                qb_v="0"
                exit
        fi
}
 
 
 
function qb_del(){
        if [ ${leeching_mode} == "true" ]
        then
                if [ ${qb_v} == "1" ]
                then
                        curl -X POST -d "hashes=${file_hash}&deleteFiles=true" "${qb_web_url}/api/v2/torrents/delete" --cookie ${cookie}
                        echo "[$(date '+%Y-%m-%d %H:%M:%S')] 删除成功！种子名称:${torrent_name}" >> ${log_dir}/qb.log
                elif [ ${qb_v} == "2" ]
                then
                        curl -X POST -d "hashes=${file_hash}&deleteFiles=true" "${qb_web_url}/api/v2/torrents/delete" --cookie ${cookie}
                else
                        curl -X POST -d "hashes=${file_hash}&deleteFiles=true" "${qb_web_url}/api/v2/torrents/delete" --cookie ${cookie}
                        echo "[$(date '+%Y-%m-%d %H:%M:%S')] 删除成功！种子文件:${torrent_name}" >> ${log_dir}/qb.log
                        echo "qb_v=${qb_v}" >> ${log_dir}/qb.log
                fi
        else
                echo "[$(date '+%Y-%m-%d %H:%M:%S')] 不自动删除已上传种子" >> ${log_dir}/qb.log
        fi
}
 
function rclone_copy(){
        if [ ${type} == "file" ]
        then
                rclone_copy_cmd=$(rclone -v copy --transfers ${rclone_parallel} --log-file  ${log_dir}/qbauto_copy.log "${content_dir}" ${rclone_dest}:/media/)
        elif [ ${type} == "dir" ]
        then
                rclone_copy_cmd=$(rclone -v copy --transfers ${rclone_parallel} --log-file ${log_dir}/qbauto_copy.log "${content_dir}"/ ${rclone_dest}:/media/"${torrent_name}")
        fi
}
 
function qb_add_auto_del_tags(){
        if [ ${qb_v} == "1" ]
        then
                curl -X POST -d "hashes=${file_hash}&tags=${auto_del_flag}" "${qb_web_url}/api/v2/torrents/addTags" --cookie "${cookie}"
        elif [ ${qb_v} == "2" ]
        then
                curl -X POST -d "hashes=${file_hash}&category=${auto_del_flag}" "${qb_web_url}/command/setCategory" --cookie ${cookie}
        else
                echo "qb_v=${qb_v}" >> ${log_dir}/qb.log
        fi
}
 
if [ -f "${content_dir}" ]
then
   echo "[$(date '+%Y-%m-%d %H:%M:%S')] 类型：文件" >> ${log_dir}/qb.log
   type="file"
   rclone_copy
   qb_login
   qb_add_auto_del_tags
   qb_del
#   rm -rf ${content_dir}
elif [ -d "${content_dir}" ]
then 
   echo "[$(date '+%Y-%m-%d %H:%M:%S')] 类型：目录" >> ${log_dir}/qb.log
   type="dir"
   rclone_copy
   qb_login
   qb_add_auto_del_tags
   qb_del
#   rm -rf ${content_dir}
else
   echo "[$(date '+%Y-%m-%d %H:%M:%S')] 未知类型，取消上传" >> ${log_dir}/qb.log
fi
 
echo "种子名称：${torrent_name}" >> ${log_dir}/qb.log
echo "内容路径：${content_dir}" >> ${log_dir}/qb.log
echo "根目录：${root_dir}" >> ${log_dir}/qb.log
echo "保存路径：${save_dir}" >> ${log_dir}/qb.log
echo "文件数：${files_num}" >> ${log_dir}/qb.log
echo "文件大小：${torrent_size}Bytes" >> ${log_dir}/qb.log
echo "HASH:${file_hash}" >> ${log_dir}/qb.log
echo "Cookie:${cookie}" >> ${log_dir}/qb.log
echo -e "-------------------------------------------------------------\n" >> ${log_dir}/qb.log
```  
注：如下图片处需要修改，/media/为我Onedrive网盘中根目录下的media文件夹，根据你的实际情况修改，注意最后的“/”一定要有.  
![](https://pic.334015.xyz/i/2025/02/03/gxekfe.png)  

赋予脚本权限  
```
chmod a+x /root/qb_auto.sh
```  

### 4. 配置qBittorrent  
在下图中所展示位置增加如下内容  
```
bash /root/qb_auto.sh  "%N" "%F" "%R" "%D" "%C" "%Z" "%I"
```  
![](https://pic.334015.xyz/i/2025/02/03/gkbpxc.png)
