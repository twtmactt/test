1.安装BBR

2.拉取v2ray镜像
```
mkdir -p /etc/v2ray
sudo docker pull v2fly/v2fly-core
cat > /etc/v2ray/config.json <<EOF
{
  "inbounds": [
    {
      "port": 21135,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "109648e3-fdc9-4b54-ae7c-8deffdbb964a",
            "alterId": 64
          }
        ]
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
EOF

sudo docker run -d --name v2ray -v /etc/v2ray:/etc/v2ray -p 21135:21135 v2fly/v2fly-core  v2ray -config=/etc/v2ray/config.json

firewall-cmd --zone=public --add-port=6666/tcp --permanent    //永久将6666端口加入开启规则
firewall-cmd --reload

查看运行状态
sudo docker container ls
```

###通过以下命令来控制 V2Ray：
```
sudo docker container start v2ray
sudo docker container stop v2ray
sudo docker container restart v2ray
```
