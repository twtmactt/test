首先安装 iptables（通常系统都会自带，如果没有就需要安装），使用以下命令  
```
sudo apt-get update
sudo apt-get install iptables
```

安装成功后使用以下命令开放一个个端口  
```
iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
```
开启一个范围的端口  
```
iptables -A INPUT -p tcp --dport 51000：60000 -j ACCEPT
```  
设置完就已经放行了指定的端口，但重启后会失效，所以需要设置持续生效规则：  

输入以下安装 iptables-persistent

```sudo apt-get install iptables-persistent```

输入以下命令保存规则持续生效
```
netfilter-persistent save
netfilter-persistent reload
```
完成之后端口就会持续开放。
