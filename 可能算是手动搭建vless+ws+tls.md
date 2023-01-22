一、更新、开放端口(443  v2ray配置端口 80 ) 安装BBR  
```
apt update
```
开启端口  https://github.com/twtmactt/test/blob/master/debian%E5%BC%80%E5%90%AF%E7%AB%AF%E5%8F%A3%E6%96%B9%E6%B3%95  
BBR      https://github.com/twtmactt/test/blob/master/%E5%BC%80%E5%90%AFbbr  

二、安装Nginx并配置  
```
apt install -y nginx
```

新建网页目录（这里在假设是/root/www）  
```
mkdir -p /root/www
```

新建首页  
```
nano /root/www/index.html
```
内容如下：  
```
<!DOCTYPE html>
<html>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" /> #解决中文乱码
<head>
<title>学习</title>
</head>
<body>
<h1>文章标题</h1>
<p><font color="red">文章内容</font></p>
<img src="此处为图片的地址">
<p>点此链接进行更多学习<a href="链接网址">链接名称</a></p>
<body style="background-color:lavender;">
<p style="background-color:indianred;">
<br>
</body>
</html>
```

新建配置文件  
注： 不同版本的nginx配置文件可能有区别，我的是nginx/1.18.0，配置文件/etc/nginx/sites-enabled/default。  
或者你的配置文件可能在/etc/nginx/conf.d/default.conf（自测是在/etc/nginx/sites-enabled/default）  
```
nano /etc/nginx/sites-enabled/default
```
如下内容(可把原来的全删了)  
```
server{
    listen 80;
    server_name 域名;
    index index.html;
    root /root/www;
}
```
```
nano /etc/nginx/nginx.conf
第一行user www-data改为user root
```

启动nginx服务  
```
systemctl start nginx
systemctl enable nginx
systemctl status nginx
```

三、申请SSL证书  
安装snapd  
```
apt-get install snapd
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

四、修改Nginx配置文件（在第一个server中添加如下内容）并重启Nginx
```
nano /etc/nginx/sites-enabled/default
location /tech
{
    proxy_pass http://127.0.0.1:你的端口号;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $http_host;
    proxy_read_timeout 300s;
}
```

五、安装V2ray  
// 安裝執行檔和 .dat 資料檔  
```
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

// 只更新 .dat 資料檔  
```
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
```
```
启动v2ray:systemctl start v2ray
设置v2ray开机自启:systemctl enable v2ray
查看v2ray的状态:systemctl status v2ray
```

六、配置v2ray并重启v2ray  
```
nano /usr/local/etc/v2ray/config.json
{
  "log": {
        "access": "/var/log/v2ray/access.log",
        "error": "/var/log/v2ray/error.log",
        "loglevel": "warning"
    },
  "inbounds": [
    {
    "port":你的端口,
      "listen": "127.0.0.1",
      "tag": "VLESS-in",
      "protocol": "VLESS",
      "settings": {
        "clients": [
          {
          "id":"你的 UUID",
            "alterId": 0
          }
        ],
            "decryption": "none"
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "path":"/tech" 
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": { },
      "tag": "direct"
    },
    {
      "protocol": "blackhole",
      "settings": { },
      "tag": "blocked"
    }
  ],
  "dns": {
    "servers": [
      "https+local://1.1.1.1/dns-query",
          "1.1.1.1",
          "1.0.0.1",
          "8.8.8.8",
          "8.8.4.4",
          "localhost"
    ]
  },
  "routing": {
    "domainStrategy": "AsIs",
    "rules": [
      {
        "type": "field",
        "inboundTag": [
          "VLESS-in"
        ],
        "outboundTag": "direct"
      }
    ]
  }
}
```

电脑端配置间图 ![image](https://photoself.eu.org/images/2022/08/23/1111.png)

