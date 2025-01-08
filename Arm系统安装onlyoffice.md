[官方文档直达链接](https://helpcenter.onlyoffice.com/installation/docs-community-install-docker-arm64.aspx)
## 新建文件  
```mkdir -p /root/onlyoffice```  
##  docker-compose配置参考   
```
version: '3'
services:
  onlyoffice:
    container_name: onlyoffice
    image: onlyoffice/documentserver-de:latest
    restart: always
    ports:
     - "8081:80"
    environment:
     - JWT_ENABLED=false
    volumes:
     - /root/onlyoffice/logs:/var/log/onlyoffice
     - /root/onlyoffice/data:/var/www/onlyoffice/Data
     - /root/onlyoffice/lib:/var/lib/onlyoffice
     - /root/onlyoffice/db:/var/lib/postgresql
```  
