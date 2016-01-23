title: ReactNative-真机运行
date: 2015-11-12 14:57:26
categories: ReactNative
tags: [第一个项目,Android,iOS]
---
<!--more-->
- iOS
	+ 在项目中运行Debug Server
	+ 根据电脑的IP，修改AppDelegate.m中的jsCodeLocation的host改为电脑的IP
	+ 或者使用`jsCodeLocation = [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"]`
- Android
	+ Android 5.0以上机型使用`adb reverse tcp:8081 tcp:8081`反向代理到Mac上
	+ Android 5.0以下选择菜单中Dev Setting > Debug Service host for device选择Mac的IP
	+ 使用`react-native run-android`启动应用
	+ 或者启动Debug Server(react-native start)
	+ 使用Android Studio进行调试
