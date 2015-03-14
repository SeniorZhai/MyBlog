title: Meteor初识
date: 2015-02-25 16:36:35
categories: node.js
tags: [node.js,meteor,web,app]
---
[Meteor](https://www.meteor.com/)是一个强大的Web APP框架，基础框架是`Node.JS`+`MongoDB`
<!--more-->
#基础
##下载安装
```shell
curl https://install.meteor.com | /bin/sh
```
##创建项目
```shell
meteor create try-meteor
```
##启动项目
```shell
cd try-meteor
meteor
```
##配置开发移动端
###安装移动端SDK
第一次需要安装SDK
```
meteor install-sdk android
meteor install-sdk ios
```
- Anroid on Mac需要以下环境
	+ [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
	+ [HAXM](https://android-bundle.s3.amazonaws.com/haxm/IntelHAXM_1.0.8.mpkg)
	+ Android SDK `meteor install-sdk android`
###配置项目
```
meteor add-platform android
meteor add-platform ios
```
###模拟器运行
```
meteor run android
meteor run ios
```
###设备上运行
```
meteor run android-device
meteor run ios-device
```
#Demo
##创建项目
```
meteor create MeteorDemo
```
##试运行
```
cd MeteorDemo
meteor
```
在浏览器中进入`localhost:3000`
![](/img/15030101.png)
	值得一提的是Meteor是热启动的，只要启动后，任何修改都是立即显示

#PS
Web技术栈
- LAMP
	- L Linux
	- A Apache
	- M MySQL
	- P PHP
- MEAN
	- M MongoDB
	- E Express.js
	- A Angular.js
	- N Node.js