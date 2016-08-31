title: guard和defer
date: 2016-08-25 19:01:12
categories: iOS
tags: [swift,2,关键字]
---
在看swift的代码的时候，遇见两个新的关键字，语法特性很特别
<!--more-->
#guard
guard有点像断言
```swift
if age < 13 {
  return
}
// 与上面代码等价
guard age >= 13 else {
  return
}
```
guard起来保证的作用，age大于13否则return
在`if-let`解包的时候使用，会使得代码看清来更简洁
```swift
guard let name = user.name else {
  return
}
/* 逻辑 */

if let name = user.name {
  /* 处理逻辑 */
} else {
  return
}
```

#defer
defer是使代码延后处理的新特性
```swift
// ...
openDirectory()
defer{
  closeDirectory()
}
opeFile()
defer {
  closeFile()
}
// ...
```
defer会在将代码块以入栈出栈的方式延后运行，比如上面的代码会先执行打开文件夹(openDirectory)后打开文件(openFile)，在所有处理完成后，执行关闭文件(closeFile)，再关闭文件夹(closeDirectory)的操作
