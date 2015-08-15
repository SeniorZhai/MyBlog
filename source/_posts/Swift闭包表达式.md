title: Swift闭包表达式
date: 2015-08-09 23:05:13
categories: iOS
tags: [Swift,闭包,lambda]
---
<!--more-->
闭包(closure)包含了可执行程序(跟方法主体(statements)类似)以及接收(capture)的参数，格式如下
```swift
{(parameters) -> return type in
	statements
}
```
- 闭包可以省略参数的类型和返回值的类型，如果省略了参数类型也要省略`in`关键字
- 闭包省略参数，可以使用`$0`,`$1`,`$2`……来引用出现的第一个、第二个、得三个……参数
- 如果闭包只包含一个表达式，那么表达式就会自动成为该闭包的返回值
```swift
func1 {
	(x:Int,y:Int) -> Int in
	return x + y
}

func2 {
	(x,y) in
	return x + y
}

func3 {return $0 + $1}

func4 { $0 + $1 }
```
