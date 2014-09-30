title: Android Studio导入Jar包及第三方开源库的方法
date: 2014-09-29 14:48:52
categories: Android
tags: [Android Studio,IDE]
---
<!--more-->
##Jar包
Jar包的处理相对简单，将Jar包复制到lib文件夹中，然后右击Jar包选择`add as a library`即可导入。
删除时，需要先删除`build.gradle`中的语句，再删除Jar包
![](/img/14092902.png)
##添加远程开源库
![](/img/14092903.png)
一般新的开源库在其README.md中都会有相应连接，比如[FlatUI](https://github.com/eluleci/FlatUI)，在其GitHub页面可以得到以下内容
![](/img/14092904.png)
要将上面内容添加到项目中的`build.gradle`中，切勿添加到全局的build.gradle文件中去
##添加本地开源库
下载开源库后，放置在`app`目录同级的目录下，编辑`setting.gradle`文件加入`:开源库文件名`，例如在app同级目录下放置了volley的开源文件夹，编辑setting.gradle的内容为
```gradle
include ':app',":volley"
```
之后再app目录下的`build.gradle`文件下，在`dependencies{}`节点下加入:
```gradle
compile project(':volley')
```