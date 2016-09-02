title: Volley
date: 2014-05-07 13:21:53
categories: Android
tags: [Android]
---
## 使用Volley

下载Volley源码并build jar包。
```
$ git clone https://android.googlesource.com/platform/frameworks/volley
$ cd volley
$ android update project -p ./
$ ant jar
```
把生成的jar包引用到项目中去。