## 1. 安装 & 配置Alist [直达链接](https://alist.nn.ci/)  

## 2. 安装Rclone  [链接地址](https://github.com/twtmactt/test/blob/master/rclone%20%E9%85%8D%E7%BD%AE%E6%96%B9%E6%B3%95.md)

## 3. 配置Rclone  
```
# 进入rclone设置
rclone config

# 选择新远程
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n #这里选择n

# 设置名字
name> remote
Type of storage to configure.
Choose a number from below, or type in your own value
[snip]
XX / WebDAV
   \ "webdav"
[snip]
Storage> webdav #这里输入远程的名字，之后就是你的远程名称

# 设置远程地址url http://your_alist_ip:port/dav
URL of http host to connect to
Choose a number from below, or type in your own value
 1 / Connect to example.com
   \ "https://example.com"
url> http://127.0.0.1:8080/dav #这里设置alist的地址和端口，后面要带dav，这是alist要求的

# 这里选6就可以了，1-5都不是我们使用的
Name of the WebDAV site/service/software you are using
Choose a number from below, or type in your own value
 1 / Fastmail Files
   \ (fastmail)
 2 / Nextcloud
   \ (nextcloud)
 3 / Owncloud
   \ (owncloud)
 4 / Sharepoint Online, authenticated by Microsoft account
   \ (sharepoint)
 5 / Sharepoint with NTLM authentication, usually self-hosted or on-premises
   \ (sharepoint-ntlm)
 6 / Other site/service or software
   \ (other)
vendor> 6

# 设置远程账号
User name
user> admin #这里是你alist的密码

# 设置远程密码
Password.
y) Yes type in my own password
g) Generate random password
n) No leave this optional password blank
y/g/n> y #这里输入y
Enter the password: #这输入你的密码，密码是看不到的
password:
Confirm the password: #再次输入你的密码
password:

# 这里直接回车即可
Bearer token instead of user/pass (e.g. a Macaroon)
bearer_token>
Remote config

# 这里可能会问你是默认还是高级，选择默认即可

# 你的远程信息
--------------------
[remote]
type = webdav
url = http://127.0.0.1:8080/dav
vendor = Other
user = admin
pass = *** ENCRYPTED ***
--------------------

# 确认
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y #输入y即可，

# 最后按q退出设置
```

## 4. 挂载到本地（再配置一下开机挂载）  
### 查看alist的目录、文件
```
rclone lsd alist:
rclone ls alist:
```
### 将alist挂载到本地目录 /webdav  
```
rclone mount alist:/ /webdav  --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000 --use-mmap
```

[原文链接](https://willxup.top/archives/deploy-alist-and-rclone)
