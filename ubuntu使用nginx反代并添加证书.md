安装nginx  
```
sudo apt-get install -y nginx
```
安装后查看nginx版本  
```nginx -v```
启动并查看状态  
```
sudo systemctl start nginx
sudo systemctl status nginx
```
创建一个新的配置文件，写下如下配置  
```nano /etc/nginx/conf.d/xxx.conf # xxx为任意名称```
配置内容  
```
server {
    listen 80;

    server_name 你的域名;

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;

        proxy_pass http://127.0.0.1:端口; #端口修改为你网站的端口
        proxy_redirect off;

        # Socket.IO Support
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```
保存退出重启  
```sudo systemctl reload nginx```

安装certbot  
```apt install certbot```

安装所需插件  
```apt install python3-certbot-nginx```

签发ssl证书  
```certbot --nginx #过程中需要填写邮箱并回答一些问题```

cerbot证书只有90天，需要更新.
