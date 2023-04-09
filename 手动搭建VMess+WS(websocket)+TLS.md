更新  
```
apt update
```

开启8443 443 80 inbounds所需端口，见iptables开启方法[跳转链接](https://github.com/twtmactt/test/blob/master/debian%E5%BC%80%E5%90%AF%E7%AB%AF%E5%8F%A3%E6%96%B9%E6%B3%95.md)  

安装v2ray（见官方脚本）  
```
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
```

编辑配置文件的内容  
```
nano /usr/local/etc/v2ray/config.json
{
    "log": {
        "access": "/var/log/v2ray/access.log",
        "error": "/var/log/v2ray/error.log",
        "loglevel": "warning"
    },
    "inbounds": [{
            "port": 11055,  #自定义端口
            "protocol": "vmess",
            "settings": {
                "clients": [{
                        "id": "",  #输入id
                        "level": 1,
                        "alterId": 0
                    }
                ]
            },
            "streamSettings": {
                "network": "ws",
                "wsSettings": {
                    "path": "/tech"  #这就不用改了
                }
            }
        }
    ],

    "outbounds": [{
            "protocol": "freedom"
        }
    ]
}
```  
检查配置是否正确  
```
/usr/local/bin/v2ray -test -config /usr/local/etc/v2ray/config.json  
```  

启动v2ray服务  
```
systemctl start v2ray
systemctl enable v2ray
```

安装nginx  
```
apt install -y nginx
```

启动nginx服务  
```
systemctl start nginx
systemctl enable nginx
systemctl status nginx
```

安装snapd  
```
apt-get install snapd
```
安装snapd core   
```
snap install core
snap refresh core
```

安装certbot并申请ssl证书  
```
snap install --classic certbot
```

创建软链  
```
ln -s /snap/bin/certbot /usr/bin/certbot
```

申请证书  
```
certbot --nginx
```

添加v2ray转发  
```
nano /etc/nginx/sites-enabled/default
```

添加一个server，加如下内容  
```
server {
    listen 8443 ssl;
    server_name yourdomain.com; // 替换为你的域名

    ssl_certificate 你的SSL证书的路径; // 替换为 SSL 证书的路径
    ssl_certificate_key 你的私钥文件的路径; // 替换为私钥文件的路径

    location / {
        proxy_pass http://127.0.0.1:11055; // 替换为 V2Ray 监听的地址和端口
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;
    }
}
```
注：11055对应/usr/local/etc/v2ray/config.json 里面inbounds端口  
   /tech客户端配置的时候需要,对应/usr/local/etc/v2ray/config.json streamSettings里的path  
   
重启nginx  
```
systemctl restart nginx
```

重启v2ray  

电脑端配置方法截图  
![image](https://photoself.eu.org/images/2022/07/26/1111.png)
