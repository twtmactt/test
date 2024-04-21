## 新建文件  
``mkdir -p /root/onlyoffice``  
## 拉取镜像并安装  
``docker pull onlyoffice/documentserver``  

``
sudo docker run -i -t -d -p 80:80 --restart=always \  
    -v /root/onlyoffice/DocumentServer/logs:/var/log/onlyoffice  \  
    -v /root/onlyoffice/DocumentServer/data:/var/www/onlyoffice/Data  \  
    -v /root/onlyoffice/DocumentServer/lib:/var/lib/onlyoffice \  
    -v /root/onlyoffice/DocumentServer/db:/var/lib/postgresql -e JWT_SECRET=false onlyoffice/documentserver
``    

这里关闭JTW

## 若出现问题，可以进行以下尝试  
### 拷贝出docker文件  
``docker cp 0ca9:/etc/onlyoffice/documentserver/local.json /root/onlyoffice``  
### 编辑文件内容如下  
``
{  
  "services": {  
    "CoAuthoring": {  
      "sql": {  
        "type": "postgres",  
        "dbHost": "localhost",  
        "dbPort": "5432",  
        "dbName": "onlyoffice",  
        "dbUser": "onlyoffice",  
        "dbPass": "onlyoffice"  
      },  
      "token": {  
        "enable": {  
          "request": {  
            "inbox": true,  
            "outbox": true  
          },  
          "browser": true  
        },  
        "inbox": {  
          "header": "Authorization",  
          "inBody": false  
        },  
        "outbox": {  
          "header": "Authorization",  
          "inBody": false  
        }  
      },  
      "secret": {  
        "inbox": {  
          "string": ""  
        },  
        "outbox": {  
          "string": ""  
        },  
        "session": {  
          "string": ""  
        }  
      }  
    }  
  },  
  "rabbitmq": {  
    "url": "amqp://guest:guest@localhost"  
  },  
  "storage": {  
    "fs": {  
      "secretString": "0pIzUjeK7RdSOKU1YfFJ"  
    }  
  }  
}  
``  
### 拷贝回docker  
``
docker cp /root/onlyoffice/local.json  0ca9:/etc/onlyoffice/documentserver
``  
### 重新启动容器
