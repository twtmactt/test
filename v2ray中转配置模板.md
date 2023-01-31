```
{
    "inbound": {
        "streamSettings": {
            "network": "ws", 
            "wsSettings": {
                "path": "/tech", 
                "headers": {
                    "Host": "vl.bfls.cf"
                }
            }
        }, 
        "protocol": "vmess", 
        "port": 12345, 
        "settings": {
            "clients": [
                {
                    "alterId": 0, 
                    "id": ""  //uuid
                }
            ]
        },
        "sniffing": {
            "enabled": true, 
            "destOverride": ["http", "tls"]
        }
    }, 
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
            },
            {
                "type": "field",
                "outboundTag": "VPS2",
                "domain": ["geosite:nba"]   //修改为nba.com
            }
        ]
    },
    "outbounds": [
        {
            "protocol": "freedom",
            "tag": "direct"
        },
        {
            "protocol": "blackhole",
            "tag": "block"
        },
        {
            "tag": "VPS2",
            "protocol": "vmess",
            "settings": {
                "vnext": [
                    {
                        "address": "",  //能看nba节点的ip
                        "port": 23456,  //能看nba节点的port
                        "users": [
                            {"id": ""}  //能看nba节点的uuid
                        ]
                    }
                ]
            }
        }
    ]
}
```
