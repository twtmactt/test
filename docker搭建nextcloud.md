1. 安装docker，docker-compose


2. 创建一下安装的目录：  
```
mkdir -p /root/data/docker_data/nextcloud

cd /root/data/docker_data/nextcloud

nano docker-compose.yml
```

3. 填入以下内容：  
```
version: "3"

services:
  nextcloud:
    container_name: nextcloud-app
    image: nextcloud:latest
    restart: unless-stopped
    ports:
      - 8080:80
    environment:
      - MYSQL_HOST=mysql
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=nextcloud
    volumes:
      - /root/data/docker_data/nextcloud/data:/var/www/html

  mysql:
    image: mysql:8.0
    container_name: nextcloud-db
    restart: unless-stopped
    environment:
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=nextcloud
      - MYSQL_ROOT_PASSWORD=nextcloud
    volumes:
      - /root/data/docker_data/nextcloud/db:/var/lib/mysql

#volumes:
#  mysql:
#  nextcloud:
```

4. 运行  
```
docker-compose up -d 
```  

5. 更新  
```
cp -r /root/data/docker_data/nextcloud /root/data/docker_data/nextcloud.archive  # 万事先备份，以防万一

cd /root/data/docker_data/nextcloud  # 进入docker-compose所在的文件夹

docker-compose pull    # 拉取最新的镜像

docker-compose up -d   # 重新更新当前镜像
```  

6. 卸载
```
cd /root/data/docker_data/nextcloud  # 进入docker-compose所在的文件夹

docker-compose down    # 停止容器，此时不会删除映射到本地的数据

rm -rf /root/data/docker_data/nextcloud  # 完全删除映射到本地的数据
``` 
