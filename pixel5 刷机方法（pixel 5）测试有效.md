刷的谷歌官方固件，使用windows10系统刷机。  
1、备份手机数据，然后打开USB调试模式（设置->系统->开发者选项->USB调试 打开）。  

2、获取 Google USB 驱动程序并安装  

官方网站：https://developer.android.com/studio/run/win-usb  

下载地址：https://dl.google.com/android/repository/usb_driver_r13-windows.zip  

下载得到的应该是usb_driver_r13-windows.zip文件，将其解压，找到android_winusb.inf，右键菜单选择安装。  

3、下载适用于 Windows 的 SDK Platform-Tools  

官方网站：https://developer.android.com/studio/releases/platform-tools.html  

下载地址：https://dl.google.com/android/repository/platform-tools_r30.0.5-windows.zip  

下载得到的应该是platform-tools_r30.0.5-windows.zip文件，将其解压。  

4、下载官方镜像文件  

官方网站：https://developers.google.com/android/images#sailfish  

下载地址：https://dl.google.com/dl/android/aosp/sailfish-qp1a.191005.007.a3-factory-d4552659.zip  

下载得到的应该是sailfish-qp1a.191005.007.a3-factory-d4552659.zip文件，将其解压。  



5、安装  

将步骤3和4解压的文件合并到一个文件夹里，文件夹名任取。  

在文件夹所在地址栏前面输入”cmd ”，效果如：cmd D:\Users\huang\Downloads\sailfish2，然后回车，就可打开cmd命令窗格，并定位到当前文件夹。  

将手机连接到电脑上，在命令窗口输入“adb reboot bootloader”，手机将自动重启并进入bootloader.  

解锁：执行“fastboot oem unlock”（fastboot flashing unlock），之后设备信息显示“unlocked”（同样方法运行fastboot flashing lock重新锁定）  

刷入系统：在文件夹内找到flash-all.bat并以管理员身份运行，等待系统自动进行刷机作业，几分钟就可以刷完了。  

手机将自动开机并进入设置程序。  
