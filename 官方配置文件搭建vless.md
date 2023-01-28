# 1.关闭防火墙，开启bbr,解析好域名

# 2.安装最新版V2ray
```
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
```

# 3.编辑配置文件的内容  
```nano /usr/local/etc/v2ray/config.json```  
```
{
  "log": {
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "port": 443,  // 若建有网站，需换成其他端口，如8443
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": ""  // 填写UUID，可以使用/usr/bin/v2ray/v2ctl uuid生成
          }
        ],
        "decryption": "none",
        "fallbacks": [
          {
            "dest": 80  // 回落配置，可以直接转到其他网站，例如"www.baidu.com:443"
          },
          {
            "path": "/websocket",  // 必须换成自定义的PATH
            "dest": 10088,
            "xver": 1
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "tls",
        "tlsSettings": {
          "alpn": [
            "http/1.1"
          ],
          "certificates": [
            {
              "certificateFile": "/etc/ssl/private/fullchain.cer",  // 换成你的证书，绝对路径
              "keyFile": "/etc/ssl/private/private.key"  // 换成你的私钥，绝对路径
            }
          ]
        }
      }
    },
    {
      "port": 10088,
      "listen": "127.0.0.1",
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": ""  // 填写你的UUID
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "network": "ws",
        "security": "none",
        "wsSettings": {
          "acceptProxyProtocol": true,  // 提醒：若使用Nginx/Caddy等反代WS，需要删掉这行
          "path": "/websocket"  // 必须换成自定义的PATH，需要和上面的一致
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom"
    }
  ]
}
```

# 4.设置开机自启并启动v2ray服务：
```
systemctl enable v2ray
systemctl restart v2ray
```

# 5.安装nginx socat
```
apt install socat nginx
```

# 6.安装证书
```
curl https://get.acme.sh | sh

alias acme.sh=~/.acme.sh/acme.sh

acme.sh --upgrade --auto-upgrade

acme.sh --set-default-ca --server letsencrypt

acme.sh --issue -d 域名 --standalone --keylength ec-256

acme.sh --install-cert -d 域名 --ecc --fullchain-file /etc/ssl/private/fullchain.cer --key-file /etc/ssl/private/private.key

chown -R nobody:nogroup /etc/ssl/private/
```

# 7.修改nginx配置文件
```
nano /etc/nginx/nginx.conf
```

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
    worker_connections 768;
}

http {

  server {
    listen 80 default_server;
    listen [::]:80 default_server;

  location / {
    proxy_pass https://www.bing.com; #伪装网址
    proxy_ssl_server_name on;
    proxy_redirect off;
    sub_filter_once off;
    sub_filter "www.bing.com" $server_name; #伪装网址
    proxy_set_header Host "www.bing.com"; #伪装网址
    proxy_set_header Referer $http_referer;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header User-Agent $http_user_agent;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto https;
    proxy_set_header Accept-Encoding "";
    proxy_set_header Accept-Language "zh-CN";
    }
  }
}
```
# 8.重新加载nginx  
```systemctl reload nginx```  

# 9.查看nginx启动状态  
```systemctl status nginx```  

# 10.重启v2ray  
```systemctl restart v2ray```  

# 11.查看v2ray启动状态  
```systemctl status v2ray```
