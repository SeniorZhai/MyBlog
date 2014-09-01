title: 理解Optional
date: 2014-07-10 13:10:42
categories: iOS
tags: [iOS,Swift]
---
Swift可以再类型后面加一个`?`来将变量声明为`optional`(随意的)。如果不是Optional的变量，那么它就必须有值，而没有值发话，我们使用Optional并且将它设置为`nil`来表示没有值。
```swift
var num:Int?
num = nil
num = 3
```
Optional Value就像一个盒子，盒子可能装着实际的值，可能声明都没装。
```swift
var num:Int?=3	// 声明一个Int的Optianal，并将其设为3
if let n = num {
	// hava a num
} else {
	// no num
}
```
使用场景
```swift
foo?.somemethod()
```