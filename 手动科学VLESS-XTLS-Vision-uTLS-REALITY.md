1. 安装xray  
```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install --beta  
```  
2. 添加并配置文件  
```  
{
  "log": {
    "loglevel": "warning"
  },
  "inbounds": [ 
    {
      "listen": "0.0.0.0",
      "port": 443,
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "",  #执行 xray uuid 生成，或 1-30 字节的字符串
            "flow": "xtls-rprx-vision"
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "show": false,
          "dest": "www.microsoft.com:443",
          "xver": 0,
          "serverNames": [ 
            "www.microsoft.com"
          ],
          "privateKey": "",  #执行 xray x25519 生成
          "shortIds": [ 
            "6ba85179e30d4fc7"   #0 到 f，长度为 2 的倍数，长度上限为 16
          ]
        }
      },
      "sniffing": {
        "enabled": true,
        "destOverride": [
          "http",
          "tls"
        ]
      }
    }
  ],
  "routing": {
    "rules": [
      {
        "type": "field",
        "protocol": [
          "bittorrent"
        ],
        "outboundTag": "blocked",
        "ip": [
          "geoip:cn",
          "geoip:private"
        ] 
      }
    ]
  },
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
    "tag": "blocked",
      "protocol": "blackhole",
      "settings": {}
    }
  ]
}  
```  
3. 开启所需端口，或者关闭防火墙
4. 使用方法 [点此直达](https://github.com/chika0801/Xray-examples/tree/main/VLESS-XTLS-uTLS-REALITY)
