Nginx 1.16.0 , Php 7.3(7.0以上) , Mysql 10.0.38-MariaDB  
1. 安装宝塔面板添加网站  
2. 下载最新的 Chevereto 程序,[点我下载](https://github.com/Chevereto/Chevereto-Free/releases)  
3. 解压后的压缩包中解压出来的 Chevereto-Free-1.1.4 文件夹下的所有文件移动到你的网站根目录里。  
4. 修改 Nginx 配置：  
   在宝塔面板中点击 网站 - 你的网站域名 - 配置文件 即可打开 Nginx 配置。  
   在 SERVER 字段加入以下代码：  
   ```
   location / {
    try_files $uri $uri/ /index.php?$query_string;
   }
   ```  
5. 添加伪静态  
```
location / {
    if (-f $request_filename/index.html){ rewrite (.*) $1/index.html break; } if (-f $request_filename/index.php){ rewrite (.*) $1/index.php; } if (!-f $request_filename){ rewrite (.*) /index.php; } try_files $uri $uri/ /api.php; } location /admin { try_files $uri /admin/index.php?$args;
    }
 ```  
或者以下  
```
location / {
try_files $uri $uri/ /index.php?$query_string;
}
location ~* \.(db3|json)$ {
  deny all;
}
location ~* ^/(temp|upload|imgs|data|application|static|system)/.*.(php|php5)$ {
    return 403;
}
```  
6. 自己配置
