title: 常用的Gradle命令
date: 2015-08-16 11:44:16
categories: Android
tags: [gradle,gradlew]
---
在Android Studio的Android项目中，都会有一个gradlew文件，代表`gradle wrapper`
在使用命令编译时，也使用`./gradlew`，使用本地的配置和环境编译
<!--more-->
# 常用的Gradle命令
- `./gradlew -v` 版本号
- `./gradlew clean` 清除build文件夹
- `./gradlew build ` 检查依赖并打包
- `./gradlew assembleDebug` 编译打包`Debug`包
- `./gradlew assembleRelease` 编译打包`Release`包
- `./gradlew installRelease` 打包并安装Release包
- `./gradlew unstallRelease` 卸载Release包
