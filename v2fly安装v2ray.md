同步时间  
```dpkg-reconfigure tzdata```  
```
apt update
apt upgrade
apt install curl
apt install nano
```
安裝執行檔和 .dat 資料檔  
```# bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)```  
//只更新 .dat 資料檔  
```# bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)```

```
cd /usr/local/etc/v2ray
rm *.json
nano config.json（粘贴需要的内容）

{
    "log": {
        "loglevel": "warning"
    },
    "routing": {
        "domainStrategy": "IPIfNonMatch",
        "rules": [
            {
                "type": "field",
                "ip": [
                    "geoip:private"
                ],
                "outboundTag": "block"
            },
            {
                "type": "field",
                "ip": [
                    "geoip:cn"
                ],
                "outboundTag": "block"
            }
        ]
    },
    "inbounds": [
        {
            "listen": "0.0.0.0", #不要改
            "port": 1234,
            "protocol": "vmess",
            "settings": {
                "clients": [
                    {
                        "id": ""
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp"
            }
        }
    ],
    "outbounds": [
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

开启bbr，开启端口  

```
systemctl enable v2ray
systemctl start v2ray
```

移除  
```
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) --remove
```  
```
firewall-cmd --zone=public --add-port=/tcp --permanent    //永久将xxx端口加入开启规则
firewall-cmd --reload
```

(for centos : systemctl enable v2ray)
$sudo apt-get remove vim-common
$sudo apt-get install vim

wget -N --no-check-certificate "https://raw.githubusercontent.com/dlxg/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
