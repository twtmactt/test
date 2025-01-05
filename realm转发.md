## 一、下载最新版本realm  
### 下载链接[点我直达](https://github.com/zhboner/realm/releases/tag/v2.7.0)
### 解压下载的文件  
```tar -xvzf 文件名```  
### 赋予权限  
```chmod +x realm```  
## 二、编辑配置文件  
### 创建realm配置文件.toml  
```nano /root/realm.toml```  
```
[network]
no_tcp = false
use_udp = true

[[endpoints]]
listen = '0.0.0.0:10001'
remote = 'test.cloudflare.com:23456' 

[[endpoints]]
listen = '0.0.0.0:10002'
remote = '1.1.1.1:23457'
```
### 创建service服务项  
```nano /etc/systemd/system/realm.service```  
```
[Unit]
Description=realm
After=network-online.target
Wants=network-online.target systemd-networkd-wait-online.service

[Service]
Type=simple
User=root
Restart=on-failure
RestartSec=5s
DynamicUser=true
ExecStart=/root/realm -c /root/realm.toml

[Install]
WantedBy=multi-user.target
```
#### 若安装位置不是root文件夹，注意修改文件中路径  
## 三、设置开启服务配置自启  
```
systemctl daemon-reload
systemctl enable realm && systemctl start realm
```
## 四、附录  
### 如果你的落地服务器使用的是动态IP，或者DDNS服务，可以在crontab计划任务里来设置定时重启realm服务：  
```
crontab -e
填写内容为时间和重启的服务名称：
00 01 * * * systemctl restart realm
00 05 * * * systemctl restart realm
00 09 * * * systemctl restart realm
00 13 * * * systemctl restart realm
00 17 * * * systemctl restart realm
00 21 * * * systemctl restart realm
```
#### 个人用记录，原文链接[链接地址](https://www.nodeseek.com/post-221885-1)
