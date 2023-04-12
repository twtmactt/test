### 1. 申请证书  
- 安装socat  
```
apt install socat
```  
- 安装证书
```
curl https://get.acme.sh | sh

alias acme.sh=~/.acme.sh/acme.sh

acme.sh --upgrade --auto-upgrade

acme.sh --set-default-ca --server letsencrypt

acme.sh --issue -d 域名 --standalone --keylength ec-256

acme.sh --install-cert -d 域名 --ecc --fullchain-file /etc/ssl/private/fullchain.cer --key-file /etc/ssl/private/private.key

chown -R nobody:nogroup /etc/ssl/private/
```  

### 2. 安装xray  
- 安装主程序
```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```  
- 更新geoip.dat and geosite.dat
```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install-geodata
```  
- 卸载命令
```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ remove
```  

### 3. 编辑配置文件  
```
nano /usr/local/etc/xray/config.json
```  
将以下内容复制粘贴，并修改端口、UUID、证书路径、密钥路径
```
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
            "listen": "0.0.0.0",
            "port": 12345, //可自定义端口
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "UUID", //可自定义ID
                        "flow": "xtls-rprx-vision"
                    }
                ],
                "decryption": "none"
            },
            "streamSettings": {
                "network": "tcp",
                "security": "tls",
                "tlsSettings": {
                    "certificates": [
                        {
                            "ocspStapling": 3600,
                            "certificateFile": "/etc/ssl/private/fullchain.cer", //自己证书文件的路径
                            "keyFile": "/etc/ssl/private/private.key"  //自己密钥的路径
                        }
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

### 4. 启动服务  
```
systemctl enable xray 
systemctl start xray
systemctl status xray
```
