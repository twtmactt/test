```
{  
    "log": {  
        "loglevel": "warning"  
    },
    "routing": {
        "domainStrategy": "AsIs",
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
                "protocol": [
                 "bittorrent"
                ],
                "ip": [
                    "geoip:cn"
                ],
                "outboundTag": "block"
            },
            {
                "type": "field",
                "domain": [
                    "nba.com"
                ],
                "outboundTag": "nba"
            }
        ]
    },
    "inbounds": [
        {
            "listen": "0.0.0.0",
            "port": 12345,
            "protocol": "shadowsocks",
            "settings": {
                "method": "aes-256-gcm",
                "password": "hello12345"
            },
            "streamSettings": {
                "network": "tcp"
            }
        },
        {
            "listen": "0.0.0.0",
            "port": 23456,
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "", // 长度为 1-30 字节的任意字符串，或执行 xray uuid 生成
                        "flow": "xtls-rprx-vision"
                    }
                ],
                "decryption": "none"
            },
            "streamSettings": {
                "network": "tcp",
                "security": "reality",
                "realitySettings": {
                    "dest": "", // 目标网站最低标准：国外网站，支持 TLSv1.3、X25519 与 H2，域名非跳转用（主域名可能被用于跳转到 www）
                    "serverNames": [ // 客户端可用的 serverName 列表，暂不支持 * 通配符，在 Chrome 里输入 "dest" 的网址 -> F12 -> 安全 -> F5 -> 主要来源（安全），填证书中 SAN 的值
                        "",
                        ""
                    ],
                    "privateKey": "wOl9GtWvkOrk6L45rJQZZkfbyxT74jos7gto1EOybFg", // 执行 xray x25519 生成，填 "Private key" 的值
                    "shortIds": [ // 客户端可用的 shortId 列表，可用于区分不同的客户端，0 到 f，长度为 2 的倍数，长度上限为 16，可留空，或执行 openssl rand -hex 1到8 生成
                        "",
                        ""
                    ]
                }
            },
            "sniffing": {
                "enabled": true,
                "destOverride": [
                    "http",
                    "tls",
                    "quic"
                ]
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
        },
        {
            "protocol": "shadowsocks",
            "tag": "nba",
            "settings": {
                "servers": [
                    {
                        "address": "1.1.1.1",
                        "port": 22222,
                        "method": "aes-256-gcm",
                        "password": "12345"
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp"
            }
        },
        {
            "protocol": "shadowsocks",
            "tag": "nba1",
            "settings": {
                "servers": [
                    {
                        "address": "2.2.2.2",
                        "port": 543219,
                        "method": "aes-256-gcm",
                        "password": "hello1234"
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp"
            }
        }
    ]
}  
```
