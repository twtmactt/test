## socks出口
```  
{
                "protocol": "socks",
                "settings": {
                "servers": [
                    {
                        "address": "ip",
                        "port": port,
                        "users": [
                            {
                                "user": "user",
                                "pass": "pass"
                            }
                        ]
                    }
                ]
            }
        }
```  

## ss出口  
```
{
            "protocol": "shadowsocks",
            "settings": {
                "servers": [
                    {
                        "address": "ip",
                        "port": port,
                        "method": "aes-256-gcm",
                        "password": "password"
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp"
            }
        }
```  

## ivacy使用https出口
[链接地址](https://github.com/twtmactt/test/blob/master/%E9%85%8D%E7%BD%AE%E8%90%BD%E5%9C%B0%E8%8A%82%E7%82%B9%E5%8F%82%E8%80%83%E6%A8%A1%E6%9D%BF.md)
