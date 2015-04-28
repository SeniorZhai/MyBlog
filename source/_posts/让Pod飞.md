title: 让Pod飞
date: 2015-04-23 22:16:07
categories: iOS
tags: [CocoaPods]
---
很久没用Pod，现在CocoaPods真是巨卡无比
<!--more-->
使用`pod install --no-repo-update`命令会好得多
也可以更换一个新的镜像
```
pod repo remove master
pod repo add master https://gitcafe.com/akuandev/Specs.git
pod repo update
```
详情见<http://akinliu.github.io/2014/05/03/cocoapods-specs-/>