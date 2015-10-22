title: Android Studio上的NDK开发
date: 2015-10-14 11:51:41
categories: Android
tags: [ndk,AndroidStudio]
---
在AndroidStudio全面支持NDK开发前，使用的方法大概是这样的<http://seniorzhai.github.io/2015/03/19/AndroidStudio%E4%B8%8A%E4%BD%BF%E7%94%A8NDK/>
在AS 1.3版本后，NDK终于可以在AndroidStudio正式支持了，不过还在实验阶段，后续可能还会有些许不同，但至少能管中窥豹了。
<!--more-->
##环境
- Android Studio 1.3+ 
- Gradle 2.5
- NDK r10e
- Build Tools 19.0.0+

##修改配置文件
- gradle/wrapper/gradle-wrapper.properties，修改distributionUrl使用Gradle2.5
```
distributionUrl=https\://services.gradle.org/distributions/gradle-2.5-all.zip
```
- build.gradle，使用gradle-experimental替代gradle
```
classpath 'com.android.tools.build:gradle-experimental:0.2.0'
```
- app/build.gradle，新版的Gradle语法有很大的改变，最外层为model，赋值使用=，新增项使用+=（具体可以在[这儿](https://github.com/SeniorZhai/AS_NDK/commit/5b4e3c8ee345360d0d0067234a87d0d35e2d096b)查看）

##创建JNI
1. 创建JNI的目录
2. 定义JNI Java类，使用javah生产头文件
3. 编写c文件
详情见<https://github.com/SeniorZhai/AS_NDK/commit/81ea7c1285e1a0ca61c79f035b3af01bff14ab11>
