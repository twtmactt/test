安装docker和docker compose  

创建answer安装目录  
```
mkdir -p /root/data/docker_data/answer
cd /root/data/docker_data/answer
nano docker-compose.yml
```
粘贴如下内容  
```
version: "3"
services:
  answer:
    image: answerdev/answer
    ports:
      - '9008:80'            # 冒号左边可以改成自己服务器未被占用的端口
    restart: on-failure
    volumes:
      - ./answer-data:/data  # 冒号左边可以改路径，现在是表示把数据存放在在当前文件夹下的 answer-data 文件夹中
      
  db:
    image: mariadb:10
    command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    environment:
      MYSQL_ROOT_PASSWORD: answer   # 数据库用户root的密码，建议自行修改一个
      MYSQL_USER: answer         
      MYSQL_PASSWORD: answer   # 数据库用户answer的密码，建议自行修改一个
      MYSQL_DATABASE: answer 
    volumes:
      - ./mariadb:/var/lib/mysql  # 冒号左边可以改路径，现在是表示把数据存放在在当前文件夹下的 mariadb 文件夹中
    restart: on-failure
```

在目录下运行  
```
cd /root/data/docker_data/answer    # 来到 dockercompose 文件所在的文件夹下
docker-compose up -d 
```

IP+端口访问即可  

更新方法：  
```
cd /root/data/docker_data/answer
docker-compose down 
cp -r /root/data/docker_data/answer/root/data/docker_data/hexo.archive  # 万事先备份，以防万一，其实这边没必要，因为我们没有映射到本地文件夹
docker-compose pull
docker-compose up -d    # 请不要使用 docker-compose stop 来停止容器，因为这么做需要额外的时间等待容器停止；docker-compose up -d 直接升级容器时会自动停止并立刻重建新的容器，完全没有必要浪费那些时间。
docker image prune  # prune 命令用来删除不再使用的 docker 对象。删除所有未被 tag 标记和未被容器使用的镜像
```

卸载方法：   
```
cd /root/data/docker_data/answer
docker-compose down
cd ..
rm -rf /root/data/docker_data/answer  # 完全删除映射到本地的数据
```
