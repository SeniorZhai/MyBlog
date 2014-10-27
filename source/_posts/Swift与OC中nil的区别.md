title: Swift与OC中nil的区别
date: 2014-10-26 22:56:37
categories: iOS
tags: [Objective-C,Swift,nil]
---
<!--more-->
在Objective-C中，nil是一个指针不存在的对象。
Swift中，nil是一个空值，不单单指指针，任何可选变量都可以被设为nil，且不能用于非可选类型。
```swift
var num : Int ? = 4	//可选类型，要么为nil要么为4
```