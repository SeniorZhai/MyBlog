title: SockMonitor和小米助手配合抓包
date: 2014-08-08 13:49:32
categories: Android
tags: [Android]
---
[SockMonitor](http://pan.baidu.com/s/1i3yxTNv)可以捕捉PC上进程的网络访问，小米助手可以为小米手机USB共享网络提供，其实可以设置IP代理的方法，但是直接这样太方便了。
## 设置
连接手机后设置共享网络，注意断开手机的3G、wifi信号
在防火墙中->允许程序或功能通过Windows防火墙->将ScockMonitor添加到其中
## 过滤
在ScockMoniter中设置过滤器通过限制`MiPoneManager`这个进程就可以只抓取手机网络访问数据
在限定IP即可抓取指定IP的数据
