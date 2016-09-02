title: NSUserDefaults
date: 2014-03-31 13:01:31
categories: iOS
tags: [iOS]
---
NSUserDefaults类提供了一个与默认系统进行交互的编程接口。
## 使用

创建一个user defaults
```objective-c
NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];
```

添加数据
```objective-c
[userDefaults setObject:nameField.text forKey:UserDefayltNameKey];
```

也可以是基本数据类型int、float、bool等，有相应的方法
```objective-c
[userDefaults setBool:YES forKey:UserDefaultBoolKey];
```

读取数据
```objective-c
[userDefaults objectForKey:UserDefaultNameKey];
[userDefaults boolForKey:UserDefaultBoolKey];
```

注：NSUserDefaults非常好用，并不需要用户在程序中设置NSUserDefaults的全局变量，需要在哪里使用NSUserDefaults的数据，那么就在哪里创建一个NSUserDefaults对象，然后进行读或者写操作。
针对同一个关键字对应的对象或者数据，可以对它进行重写，重写之后关键字就对应新的对象或者数据，旧的对象或者数据会被自动清理。