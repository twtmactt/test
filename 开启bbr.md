五合一的TCP网络加速脚本，包括了BBR原版、BBR魔改版、暴力BBR魔改版、BBR plus（首选）、Lotsever(锐速)安装脚本。  
可用于KVMXen架构，不兼容OpenVZ（OVZ）。支持Centos 6+ / Debian 7+ / Ubuntu 14+，BBR魔改版不支持Debian 8。  
```
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh"
chmod +x tcp.sh
./tcp.sh
```

使用标准方式在 Ubuntu 16.04 下启用 TCP 拥塞控制之 BBR  
为 Ubuntu 16.04 安装 4.10 + 新内核  
```
sudo apt install linux-generic-hwe-16.04
```

安装好以后重启电脑，然后输入：  
```
uname -a     看看是不是变成 4.10 内核了
```

为 Ubuntu 16.04 启用 BBR  
```
sudo modprobe tcp_bbr
echo "tcp_bbr" | sudo tee -a /etc/modules-load.d/modules.conf
```
装载后，再执行 
```sysctl net.ipv4.tcp_available_congestion_control ```
你就可以看到 BBR 出现在输出结果里了。  

接下去再正式启用它：  
```
echo "net.core.default_qdisc=fq" | sudo tee -a /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

执行完这几条指令后，再用 sysctl net.ipv4.tcp_congestion_control 验证一下，看到返回结果是：net.ipv4.tcp_congestion_control = bbr


Debian9/10开启bbr  
1、修改系统变量  
```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
```

2、保存生效  
```
sysctl -p
```
3、查看内核是否已开启BBR   
```
sysctl net.ipv4.tcp_available_congestion_control
```

显示以下即已开启：
sysctl net.ipv4.tcp_available_congestion_control  
net.ipv4.tcp_available_congestion_control = bbr cubic reno

4、查看BBR是否启动  
```
lsmod | grep bbr
```
显示以下即启动成功：
lsmod | grep bbr
tcp_bbr                20480  14
