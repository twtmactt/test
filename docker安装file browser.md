1.安装docker  
2.下载filebrowser镜像  
```
docker pull filebrowser/filebrowser
```
3.创建filebrowser挂载所需要的目录
```
mkdir /root/filebrowser
```
4.启动filebrowser
```
docker run -d -v /root/downloads:/srv -v /root/filebrowser/filebrowserconfig.json:/etc/config.json -v /root/filebrowser/database.db:/etc/database.db --name myfile -p 8081:80 filebrowser/filebrowser:s6
```

默认用户名密码：```admin```  

官方文档：https://filebrowser.org/installation
5.修改nginx配置文件  
```
nano /etc/nginx/nginx.conf 
```  
添加如下内容  
```
client_max_body_size 0;
proxy_read_timeout 999999s;
```

使用docker-compose配置文件参考  
```
version: '3'
services:
    filebrowser:
        container_name: filebrowser
        ports:
            - '5993:80'   # 5993可以改成任意vps上未使用过的端口，80不要改
        volumes:
            - /path/to/root:/srv
            - /path/to/filebrowser.db:/database/filebrowser.db
            - /path/to/settings.json:/config/settings.json
        environment:
            - PUID=0    # 稍后在终端输入id可以查看当前用户的id
            - PGID=0    # 同上
            - TZ=Asia/Shanghai
        restart: always
        image: filebrowser/filebrowser
```
