1.首先开启usb调试，然后用数据线连接电脑和手机。  
2.下载适用于 Windows 的 SDK Platform-Tools，并解压.  
   [官方网站直达](https://developer.android.com/studio/releases/platform-tools.html)      
 
 下载得到的应该是platform-tools_r30.0.5-windows.zip文件（版本可能不同）  
    
3.将里面名称中含有adb.exe和fastboot.exe都复制到 c:/windows/system32

   将名称中含有adb的所有文件复制到 c:/windows/system
   
   将所有名字含adb的文件放Windows/SysWOW64下
   （32位系统执行前两个即可）
   
4.在电脑开始菜单-运行 输入cmd，打开命令提示符，依次输入下面语句（支持android 11）

 ```
   adb shell settings put global captive_portal_mode 0
   adb shell settings put global captive_portal_https_url https://www.google.cn/generate_204
```

5.开启、关闭飞行模式

备用方法：

1、关机-开机-连上wifi

2、设置-用户-添加新用户

3、此时会出现和刚刷机完以后那样需要网络连接，一直点下一步，让手机连接网络

4、关键，当进行到正在联网时，强制关机

5、开机

6、切换下网络，你会发现感叹号很快消失了

7、删除刚刚新添加的用户
