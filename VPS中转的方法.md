两台VPS：VPS1为常用不解锁机，VPS2为流媒体解锁机  
1、在VPS2安装v2ray，开启端口，安装BBR  
2、在VPS1上搭建VMess+WS(websocket)+TLS    
步骤参考https://github.com/twtmactt/test/blob/master/%E6%89%8B%E5%8A%A8%E6%90%AD%E5%BB%BAVMess%2BWS(websocket)%2BTLS  
修改部分如下：
编辑配置文件的内容改为
```
{
    "log": {
        "loglevel": "warning",
        "access": "/var/log/v2ray/access.log",
        "error": "/var/log/v2ray/error.log"
    },
    "routing": {
        "domainStrategy": "AsIs",
        "rules": [
            {
                "type": "field",
                "ip": ["geoip:private"],
                "outboundTag": "block"
            }
        ]
    },
    "inbound": {
        "streamSettings": {
            "network": "ws",
            "wsSettings": {
                "path": "/tech",
                "headers": {
                    "Host": ""  #vps1的绑定域名
                }
            }
        }, 
        "protocol": "vmess",
        "port": 10085,  #vps1的端口
        "settings": {
            "clients": [
                {
                    "alterId": 0,
                    "id": "8bd3d957-626a-4d62-be7a-5bc272557f00"  #vps1的id
                }
            ]
        }
    },
    "outbounds": [
        {
            "protocol": "vmess",
            "settings": {
                "vnext": [
                    {
                        "address": "sg1.eveaz.com",  #vps2的IP
                        "port": 10086,  #vps2的端口
                        "users": [
                            {"id": "9efb0601-54d8-4eac-a43a-662fa51fca1a"}  #vps2的id
                        ]
                    }
                ]   
            }
        },
        {
            "protocol": "freedom",
            "tag": "direct"
        },
        {
            "protocol": "blackhole",
            "tag": "block"
        }
    ]
}
```  
nginx配置文件添加部分为  
```
location /tech {
      proxy_redirect off;
      proxy_pass http://127.0.0.1:10085;  #vps1的端口
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $host;
      # Show real IP in v2ray access.log
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
```
