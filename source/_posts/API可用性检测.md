title: API可用性检测
date: 2016-08-26 13:37:00
categories: iOS
tags: [Swift]
---
Swift有检查API可用性的内置支持，确保我们在使用时不会不小心使用当前不可用的API
<!--more-->
```swift
if #available(platform name version, ..., *) {
  statements
} else {
  statements
}
```
可用性条件获取了一系列平台的名字和版本，平台名可以是`iOS`、`OSX`、`watchOS`，版本号可以是主版本号或者小版本，比如
```swift
#available(iOS 9, OSX10.10, *)
```
最后一个参数`*`是必须写的，用于处理未来潜在的平台
