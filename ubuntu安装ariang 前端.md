宝塔面板＋建立网站+上传zip文件  
DOCKER 配置  
1.安装DOCKER  
更新包列表  
```sudo apt update```  
安装必须的包  
```sudo apt install apt-transport-https ca-certificates curl software-properties-common```  
为国内的 azure 仓库添加 GPG Key  
```curl -fsSL https://mirror.azure.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -```  
添加 docker 仓库到 Apt 源  
```sudo add-apt-repository "deb [arch=amd64] https://mirror.azure.cn/docker-ce/linux/ubuntu bionic stable"```  
再次更新包列表  
```sudo apt update```  
安装 docker  
```sudo apt install docker-ce```  
验证 docker 安装是否成功  
```docker --version```  
安装 docker compose （可选）  
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
部署 Ar­i­aNg 页面  
```
docker run -d \
    --name ariang \
    --restart unless-stopped \
    --log-opt max-size=1m \
    -p 6880:6880 \
    p3terx/ariang
```
