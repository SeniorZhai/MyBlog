title: 魅族手机连接Mac的问题
date: 2015-06-24 14:12:55
categories: Android
tags: [魅族,连接,Mac]
---
部分三星、魅族的手机，连接Mac时，执行`adb devices`不能发现设备
<!--more-->
#解决方法
1. 打开`系统信息`，在USB中找到手机，然后查找厂商ID
2. 在`~/.android/adb_usb.ini`文件中添加厂商ID
3. 执行`adb kill-server`和`adb start-server`重启ADB
![](/img/15062001.png)