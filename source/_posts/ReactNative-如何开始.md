title: ReactNative-如何开始
date: 2015-09-24 17:00:31
categories: ReactNative
tags: [React,开始,Android,iOS]
---
<!--more-->
# 基本环境
1. Mac OS X
2. Node.js 4.0
3. Xcode 6.3以上版本
4. Android SDK
5. watchman和flow
## 安装
```shell
npm install -g react-native-cli
```
## 初始化
```shell
react-native init AwesomeProject
```
## 开发环境配置
- iOS XCode6.3及以上即可
- Android
	+ 环境变量`ANDROID_HOME`
	+ SDK Manager
		- Android SDK Build-tools version 23.0.1
		- Android 6.0(API 23)
		- Android Support Repository

## 运行
- iOS
在Xcode中打开工程，直接运行即可
- Android
在`./android/`目录下新建`local.properties`文件
把SDK的路径保存在其中
```
sdk.dir=.../Android/sdk
```
执行命令
```shell
react-native run-android
```
![](/img/15092401.png)
![](/img/15092402.png)
<https://github.com/SeniorZhai/ReactNative_Demo>
