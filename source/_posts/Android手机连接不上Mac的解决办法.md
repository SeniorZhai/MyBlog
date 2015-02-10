title: Android手机连接不上Mac的解决办法
date: 2015-01-27 13:40:54
categories:
tags:
---
<!--more-->
1. 将Android手机的调试模式打开
2. 按住选择Mac的option键，选择Apple图标选项的`系统信息`
3. 找到USB，查看手机的厂商ID，比如我的MX Pro是`0x2a45`
![](/img/15012701.png)
4. 在终端中输入并执行`echo 0x2a45 >> ~/.android/adb_usb.ini`
5. 重启ADB服务（adb restart或者重启Eclipse、AndroidStudio）