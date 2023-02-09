## 安装依赖（若是debian）  
```
apt install  
apt install wget curl sudo vim git -y
```  

## 设置SWAP  
```
wget -O box.sh https://raw.githubusercontent.com/BlueSkyXN/SKY-BOX/main/box.sh && chmod +x box.sh && clear && ./box.sh
```

## 安装docker,docker-compose  
   [点我查看安装方法](https://github.com/twtmactt/test/blob/master/Docker%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4.md)  
  

## 创建安装目录及docker-compose文件  
```
sudo -i
mkdir -p /root/data/docker_data/fireshare
cd /root/data/docker_data/fireshare
nano docker-compose.yaml  
```  

写入下面的文件：  
```
version: '3.7'
services:
  fireshare:
    container_name: fireshare
    image: shaneisrael/fireshare:latest    # latest表示最新版本
    ports:
      - "8080:80"        # 冒号左边的端口可以修改
    volumes:
      - ./data:/data         # 冒号左边的路径可以自己修改（./代表当前目录下），冒号右边不要改！
      - ./processed:/processed 
      - ./videos:/videos  # 可以改为rclone挂载地址
    environment:
    - ADMIN_USERNAME=admin    
    - ADMIN_PASSWORD=admin    
```

## 新建文件夹fireshare和子目录  
```
mkdir -p /root/data/docker_data/fireshare/{data,processed,videos} #根据实际情况修改  
```

## 运行  
```docker-compose up -d```  

## 使用nginx反代并添加证书  
nginx配置如下：  
```
server {
    listen       80;
    server_name  域名;

    location / {
      proxy_pass http://ip:端口;  #docker容器内ip和映射端口(ip addr show docker0)       
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
    }
}
```  
## 更新  
```
cp -r /root/data/docker_data/fireshare /root/data/docker_data/fireshare.archive  # 备份
cd /root/data/docker_data/fireshare  
docker-compose pull    # 拉取最新的镜像
docker-compose up -d   # 重新更新当前镜像
```  

## 卸载  
```
cd /root/data/docker_data/fireshare  
docker-compose down    
rm -rf /root/data/docker_data/fireshare 
```  







