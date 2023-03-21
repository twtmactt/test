### 配置如下，修改两处 proxy_pass http://ip:port  
```
#PROXY-START/
location ~* \.(gif|png|jpg|css|js|woff|woff2)$
{
    proxy_pass http://ip:port; #这里
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    expires 12h;
}
client_max_body_size 5000M;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For '$proxy_add_x_forwarded_for';
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Sec-WebSocket-Extensions $http_sec_websocket_extensions;
    proxy_set_header Sec-WebSocket-Key $http_sec_websocket_key;
    proxy_set_header Sec-WebSocket-Version $http_sec_websocket_version;
    proxy_cache off;
    proxy_redirect off;
    proxy_buffering off;
location / {
        proxy_pass http://ip:port; #这里
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_ssl_verify off;
        proxy_http_version 1.1;
        proxy_set_header Host $http_host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 86400;
    }
 
#PROXY-END/

```
