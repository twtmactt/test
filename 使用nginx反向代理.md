## 1.安装nginx  
```
apt install -y nginx  
systemctl start nginx
systemctl enable nginx
systemctl status nginx
```

## 2.修改配置文件  
注： 不同版本的nginx配置文件可能有区别，我的是nginx/1.18.0，配置文件/etc/nginx/sites-enabled/default。  
或者你的配置文件可能在/etc/nginx/conf.d/default.conf（自测是在/etc/nginx/sites-enabled/default）
```
nano /etc/nginx/sites-enabled/default
```
## 3.删除原内容，添加如下内容  
```
server {
    listen       80;
    server_name  绑定的域名;# 服务器地址或绑定域名

    location / {
      proxy_pass http://127.0.0.1:9008/;       # 注意改成你实际使用的端口
      rewrite ^/(.*)$ /$1 break;
      proxy_redirect off;
      proxy_set_header Host $host;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Upgrade-Insecure-Requests 1;
      proxy_set_header X-Forwarded-Proto https;
    }
}
```
