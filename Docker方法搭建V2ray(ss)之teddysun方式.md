1.安装BBR  
2.安装Docker  

3.拉取v2ray镜像  
```
docker pull teddysun/v2ray
mkdir -p /etc/v2ray
cat > /etc/v2ray/config.json <<EOF
{
  "inbounds": [{
    "port": 9000,（自定）
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "",
          "level": 1,
          "alterId": 64
        }
      ]
    }
  }],
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  }]
}
EOF
docker run -d -p 9000:9000 --name v2ray --restart=always -v /etc/v2ray:/etc/v2ray teddysun/v2ray
```

```
firewall-cmd --zone=public --add-port=6666/tcp --permanent    //永久将6666端口加入开启规则
firewall-cmd --reload
```
###通过以下命令来控制 V2Ray：  
```
sudo docker container start v2ray
sudo docker container stop v2ray
sudo docker container restart v2ray
```

拉取ss镜像  
```
docker pull teddysun/shadowsocks-libev
mkdir -p /etc/shadowsocks-libev
cat > /etc/shadowsocks-libev/config.json <<EOF
{
    "server":"0.0.0.0",
    "server_port":9000,
    "password":"password0",
    "timeout":300,
    "method":"aes-256-gcm",
    "fast_open":true,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp"
}
EOF
docker run -d -p 9000:9000 -p 9000:9000/udp --name ss-libev --restart=always -v /etc/shadowsocks-libev:/etc/shadowsocks-libev teddysun/shadowsocks-libev
```
