title: AVOS Cloud
date: 2014-04-01 13:02:01
categories: iOS
tags: [iOS]
---
在Podfile中添加
```
pod 'AVOSCloud'
```
使用UI相关的相关功能，就添加
```
pod 'AVOSCloundUI'
```
SNS组件的相关功能，就添加
```
pod 'AVOSCloundSNS'
```

在`AppDelegate.m`文件，添加下列导入语句到头部
```objective-c
# import <AVOSClound/AVOSCloud.h>;
```
在`application:didFinishLaunchingWithOptions`函数内添加
```objective-c
[AVOSClound setApplicationId:@"" clientKey:@""];
```
要跟踪应用的打开情况，添加下列代码
```
[AVAnalytics trackAppOpenedWithLaunchOptions:launchOptions];
```

## 对象
AVObject

在AVOS Cloud上，数据存储是围绕`AVObject`进行的。每个`AVObject`都包含了与JSON兼容的key-value对应的数据。数据是schema-free的，不需要在每个AVObject上提前制定存在哪些键，只要直接设定对应的key-value即可。
key必须是字母数字或下划线组成的字符串，自定义的键不能以`__`开头。值可以是字符串、数字、布尔值、甚至是数组和字典。
**注意：在iOS SDK中，`uuid`是保留字段，不能作为key来使用。**

### 保存对象

```objective-c
AVObject *gameScore = [AVObject objectWithClassName:@"GameScore"];
[gameScore setObject:[NSNumber numberWithInt:1337] forKey:@"score"];
[gameScore setObject:@"Steve" forKey:@"playerName"];
[gameScore setObject:[NSNumber numberWithBool:NO] forKey:@"cheatMode"];
[gameScore save];
```