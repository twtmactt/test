
搭建方法[官方文档](https://jellyfin.org/docs/general/installation/container)
通过docker安装：
- 先切换到root用户
```html
sudo -i
```
安装docker docker-compose
```html
curl -sSL https://get.docker.com/ | sh
systemctl start docker 
systemctl enable docker
apt-get install docker-compose
```  
创建目录（这个目录是用来挂载网盘的，同时也是后面docker映射目录）
```html
mkdir /root/media #文件名任取，这里是media
```  
创建并进入jellyfin安装目录
```html
mkdir -p /data/docker_data/jellyfin
cd /data/docker_data/jellyfin
```  
创建Docker Compose文件
```html
nano docker-compose.yml
```  
在文件中输入以下内容：
```bash
version: "2.1"
services:
  jellyfin:
    image: lscr.io/linuxserver/jellyfin:latest
    container_name: jellyfin
    environment:
      - PUID=0
      - PGID=0
      - TZ=Asia/Shanghai
    volumes:
      - /data/docker_data/jellyfin/config:/config
      - /data/docker_data/jellyfin/cache:/cache
      - /root/meida:/media
    ports:
      - 8096:8096
      - 8920:8920 #optional
      - 7359:7359/udp #optional
      - 1900:1900/udp #optional
    restart: unless-stopped
```  
保存并退出文件 `ctrl+x`然后 `y`
启动Jellyfin
```html
docker-compose up -d
```  
使用ip:8096访问你的jellyfin服务器

简单配置 
- 登录服务器后，首先到Dashboard—Users—Open—Profile中，把这个勾去掉

 ![3](https://photoself.eu.org/images/2023/03/21/image42db61af47f0c840.png)


- 然后点击Libraries添加内容。
 ![4](https://photoself.eu.org/images/2023/03/21/imagee937b23843fa755c.png)


- 再到Plugins—Repositories中添加如下内容（下载字幕）  
```html
https://raw.githubusercontent.com/josdion/subbuzz/master/repo/jellyfin_10.8.json
```  

- 因为docker搭建后，运行jellyfin会导致占用cpu很高，通过以下方法设置一下  
停止容器  
```html
docker stop 容器名
```  
限制cpu使用  
```html
docker container update  容器名  --cpus="2" #限制cpu使用2核
```  
启动容器  
```html
docker start 容器名
```  

- 还会遇到一个问题，就是中文字幕显示乱码的情况，解决办法如下  
首先查看容器ID
```html
docker ps
```  
进入容器  
```html
sudo docker exec -it 容器id /bin/bash
```  
依次输入以下命令：  
```html
apt update
apt install fonts-noto-cjk-extra
```  
退出容器  
```html
exit
```  
重启容器  
```html
docker restart 容器ID
```  

添加中文备用字体，我的config映射目录为 `/data/docker_data/jellyfin/config`，所以后面都以此为例，你自己的要做相应修改  

　先下载字体，[下载地址](https://github.com/CodePlayer/webfont-noto/raw/master/release/NotoSansCJKsc-hinted-standard.zip)，[备用地址](https://drive.google.com/file/d/1dqAoSB9Tec-A1JDiIASSAOs_njQKy8fn/view?usp=share_link)  
 
　在/data/docker_data/jellyfin/config中创建一个文件夹  
```html
mkdir fonts
```  
　将字体中 **NotoSansCJKsc-Medium.woff2** 这个文件复制到/data/docker_data/jellyfin/config/fonts文件夹中  

　在控制台-播放中设置启用备用字体  
 
 ![Test](https://photoself.eu.org/images/2023/03/27/image.png)   

 
 最后重启一下jellyfin  
 
 如果是搭建emby，方法类似，docker-compose文件如下（使用lovechen镜像）  
```
 version: "2.3"
services:
  emby:
    image: lovechen/embyserver:latest
    container_name: emby
	network_mode: bridge
    environment:
      - PUID=0
      - PGID=0
      - TZ=Asia/Shanghai
    volumes:
      - /data/docker_data/emby/config:/config
      - /data/docker_data/emby/cache:/cache
      - /root/media:/media
    ports:
      - 8096:8096
      - 8920:8920 #optional
    restart: unless-stopped  
```


















