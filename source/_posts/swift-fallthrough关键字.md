title: swift-fallthrough关键字
date: 2015-04-23 22:01:34
categories: iOS
tags: [swfit,switch,fallthrough]
---
fallthrough关键字在`switch case`中使用，为了使得swift能够保持C语言中switch的语法特性
<!--more-->
总所周知，在大多数语言中，switch语句块，case要紧跟break，否则case之后的语句会顺序运行
```c
int i = 1;
switch(1){
	case 1:
		i++;
	case 2:
		i++;
	case 3:
		i++;
	default:
		i++;
}
// i = 5
```
如果在swift中，默认是不会执行下去的
```swift
var i : Int = 1

switch 1{
case 1:
    i++
case 2:
    i++
case 3:
    i++
default:
    i++
}

// i = 2
```
为了保持这种特性可以使用`fallthrough`关键字
```swift
var i : Int = 1

switch 1{
case 1:
    i++
    fallthrough
case 2:
    i++
    fallthrough
case 3:
    i++
    fallthrough
default:
    i++
}
```
