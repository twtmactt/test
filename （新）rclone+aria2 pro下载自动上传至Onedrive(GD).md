（新）rclone+aria2 pro下载自动上传至Onedrive(GD)  
1.安装docker并开启端口  

2.客户端授权（上传GD跳过这一步）  
在本地Windows电脑上下载rclone，[下载地址](https://rclone.org/downloads/)。  
然后解压出来，比如我解压到D盘，文件夹命名rclone，此时点击Win+R，然后输入cmd，确定。再输入以下命令：  
```
cd /d d:\rclone
rclone authorize "onedrive"
```

复制token:
{"access_token":"xxxx"}  
<---End paste    #请复制{xx}整个内容，后面需要用到


3.安装aria2，最基本的启动命令如下，你只需要完整替换<TOKEN>字段(RPC密钥)即可启动。  
```
docker pull p3terx/aria2-pro

docker run -d \
    --name aria2-onedrive \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=xxxxx \
    -e RPC_PORT=6803 \
    -e LISTEN_PORT=33333 \
    -v ~/aria2-onedrive-config:/config \
    -v ~/onedrive-downloads:/downloads \
    -e SPECIAL_MODE=rclone \
    p3terx/aria2-pro
```
	
4.安装rclone（可以跳过？）  
```
curl https://rclone.org/install.sh | sudo bash
```
	
5.添加网盘（以onedrive为例子）  
```
docker exec -it aria2-onedrive rclone config
```
	
6.之前若使用过 RCLONE 直接把配置文件（rclone.conf）复制到 Aria2 Pro 配置目录下即可。 RCLONE 配置文件可以在宿主机的默认位置找到：~/.config/rclone/rclone.conf
最后根据实际情况修改 Aria2 Pro 配置文件目录下script.conf文件中的网盘名称(drive-name)和网盘路径(drive-dir)这两个选项的值。
