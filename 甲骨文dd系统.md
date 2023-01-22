#更新apt源  
```
apt-get update
```

#安装需要的工具包  
```
apt-get install -y xz-utils openssl gawk file
```

然后执行以下脚本，脚本全自动运行，dd之后会造成断开链接的情况，不用担心，请耐心等待20分钟或更久。可以通过ping端口来检测是否DD完成。
```
Debian 9
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -d 9 -v 64 -a -firmware

Debian10
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -d 10.3 -v 64 -a -firmware

Debian11
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -d 11 -v 64 -a -firmware
```

DD安装完毕之后，请立即更新密码。默认用户名为：root，默认密码为：MoeClub.org

等待Oralce自行DD完成之后（大概15-30分钟），通过tcp.ping.pe此工具来判断是否DD完成

修改密码
passwd
