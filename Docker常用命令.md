
安装Docker(注意amd和arm是否一致)  
法一：安装docker   
## ubuntu/debian 系统: 
```
curl -sSL https://get.docker.com/ | sh
snap install docker
```
## Centos 7 系统：  
```
wget -qO- get.docker.com | bash 
 ```  
法二：官方https://docs.docker.com/engine/install/  

3.启动docker/允许开机启动  
 ```  
systemctl start docker 
systemctl enable docker 
```

安装docker-compose(注意amd和arm是否一致)  
```
apt-get install docker-compose
docker-compose --version
```

设置自动重启  
1创建容器时设定  
```
docker run -d --restart=always --name 容器名 使用镜像
```
重启策略 --restart 具体参数  
no  //默认策略，容器退出时不重启  
on-failure  //非正常退出时，才重启  
on-failure:3  //非正常退出时，才重启，最多三次  
alwayls  //无论推出状态如何，都重启  

2修改已有容器  
```
docker update --restart=always 容器id或容器名
```


删除docker镜像  
1、查看命令  ```docker images```  
2、删除命令  ```docker image rm [image]```

现有镜像重做tag并上传到自己的Repository（需要先登录docker）  
```
docker login
#语法说明 docker tag local-image:tagname new-repo:tagname
#语法说明 docker push new-repo:tagname
#实践，做tag
docker tag 已有REPOSITORY:tag 想要改的REPOSITORY:tag
#实践，push镜像到docker hub repository
docker push  想要改的REPOSITORY:tag

docker镜像下载到本地（home位置可以更改）
docker save imageid > /home/容器名.tar
```
