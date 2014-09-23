title: Android、iOS大不同(一)
date: 2014-08-09 13:54:07
categories: iOS
tags: [Android,iOS,Java,Objective-C,大不同]
---
长夜漫漫，无心睡眠，定一个小目标吧，写一下自己对android、iOS开发区别的见解。
<!--more-->
##Java、OC大不同
本人先会的Java，所以比较都会以Java、Android入手开始比较。
##1. 类的定义
OC的类定义比较特殊分为两个部分，接口部分和实现部分
比如一个MyClass类，有岁数和名字两个成员变量，一个实例方法jump和一个类方法run
java是这么定义的
```java
public class Person {
	public int age;
	public String name;
	private long tmp;
	public Person(int age,String name){
		this.age = age;
		this.name = name;
	}
	public void jump(String str){
		//
	}
	public static void run(){
		//
	} 
}
```

OC的定义需要两个部分`MyaClass.h`和`MyClass.m`
    
```objective-c
#import <Foundation/Foundation.h>

@interface MyClass : NSObejct
{
	int _age;
	NString* _name;
}
- (void)jump:(NSString*)str;
+ (void)run;
@end
```

```objective-c
#import "MyClass.h"

@implementation MyClass
{
	long _tmp; // 只能在实现部分使用，相当于private的变量 
}
- (void)jump:(NSString *)str
{
	//
}
+ (void)run
{
	//
}
@end
```

##2.新建对象和使用
如果java中的对象是这样的
```java
public class MyClass{
	public void run(){
		System.out.println("I'm running!!!");
	}
}
```

它的新建就是`MyClass a = new MyClass()`，Java帮助它实现了默认的一个构造函数。使用run方法时则是`a.run()`。
OC则是这样的

```
// MyClass.h
...
@interface MyClass : NSObject
- (void)run;
@end
// MyClass.m
@implementation MyClass
- (void)run
{
	NSLog(@"I'm running!!!");
}
```
使用时`MyClass a = [[MyClass alloc] init]`，OC同样帮助它实现了一个默认的构造函数。使用run方法时则是`[a run]`。