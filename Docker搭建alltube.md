1. 安装docker和docker compose  
   [教程地址](https://github.com/twtmactt/test/blob/master/Docker%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4.md)  

2. 创建安装目录  
```
mkdir -p /root/data/docker_data/alltube

cd /root/data/docker_data/alltube

nano docker-compose.yml  
```  
3.添加文件并编辑  
```
version: '3.3'
services:
    alltube:
        container_name: alltube
        ports:
            - '5993:80'   # 5993可以改成任意vps上未使用过的端口，80不要改
        environment:
            - PUID=0    # 稍后在终端输入id可以查看当前用户的id
            - PGID=0    # 同上
            - TZ=Asia/Shanghai
        restart: always
        image: rudloff/alltube
```
4.启动容器  
```
docker-compose up -d 
```  

5.更新  
```
cd /root/data/docker_data/alltube

docker-compose down 

cp -r /root/data/docker_data/alltube /root/data/docker_data/alltube.archive  # 万事先备份，以防万一，其实这边没必要，因为我们没有映射到本地文件夹

docker-compose pull

docker-compose up -d 

docker image prune  # prune 命令用来删除不再使用的 docker 对象。删除所有未被 tag 标记和未被容器使用的镜像
```  

6.卸载  
```
docker stop alltube

docker rm -f alltube  # 停止容器，此时不会删除映射到本地的数据

rm -rf /root/data/docker_data/alltube  # 完全删除映射到本地的数据
```

