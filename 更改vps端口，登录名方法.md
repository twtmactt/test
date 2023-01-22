1.改端口  
```
nano /etc/ssh/sshd_config
```
在打开的文件中找到Port这一项，并修改它的端口  
   使用 ctrl+w 进入搜索模式，然后输入 Port 22 并回车  
   删除 22 并改成 9753  
   说明：如果这一行开头有个#，证明这一行【不生效】（被注释掉了），你可像我一样在文件最后写一个不带#的，或者把#删掉就好。  

重启ssh服务，使变更生效  
```
systemctl restart ssh
```

2.建立非root的新用户（vpsadmin为用户名）    
新增一个用户并设定登录密码  
```
adduser vpsadmin
```


安装sudo功能（可能不用）  
```
apt update && apt install sudo
```

把vpsadmin用户加入sudo名单里  
```
visudo
```
在 User Priviledge Specification 下加入一行 vpsadmin ALL=(ALL) NOPASSWD: ALL 即可。  

3.禁用root用户SSH远程登录  
```
nano /etc/ssh/sshd_config
```
找到PermitRootLogin Yes这一项，然后把它后面的设定值改为no即可  

重启ssh服务  
```
systemctl restart ssh
```
