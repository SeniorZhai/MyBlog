title: Android、iOS大不同(二)
date: 2014-09-01 19:04:07
categories: iOS
tags: [Android,iOS,Java,Objective-C,大不同]
---
面向对象的一些知识点
<!--more-->
## 代表对象自己的
Java使用`this`表示对象自己
```java
this.age = 10;
this.run();
```
而Objective-C使用`self`代表
```objective-c
self->age = 10;
[self run];
```
## 通用类型
JAVA中，所有类都默认继承Object，所以可以用Object代表所有对象的类型。
Objective-C中，所有对象都继承NSObejct，而且Objective-C提供了一个id类型，可以代表所有对象的类型。
当通过id类型的变量来跟踪对象的所属的类，它会在运行时判断该对象所属的类，并在运行时确定需要的动态调用方法。
```objective-C
id p = [[Person alloc] init];
[p say:@"你好!"];
```
## 方法
Obejective-C和Java中，类是一等公民，所有的方法都不能独立存在，方法必须属于类或者对象。
在Java中，被`static`限定的就是类方法;Objective-C中，类方法用`+`标识。
- 方法只能在类中定义，不能单独存在
- 方法要么属于类本身，要么属于类的一个对象
- 永远不能单独执行方法，执行方法必须使用类或者对象作为调用者

## 参数可变的方法
在java中，最后一个参数的变量类型后增加`...`即可表示方法接受可变参数
参数可以视为一个数组处理
```java
public int add(int x,int... args)
{
	int sum = x;
	for(int i = 0; i < args.length ; i ++)
	{
		sum += args[i];
	}
	return sum;
}
```
在Obejctive-C中，最后一个形参后增加`,...`(逗号三点)表示方法可接受对个参数值
为了获取可变形参，可以使用如下关键字
- va_list 类型，定义可变参数列表的指针变量
- va_start 函数，该函数指定开始处理可变形参的列表，并让指针变量指向可变形参列表的第一个参数
- va_end 函数，结束处理，释放指针变量
- var_arg 函数，返回获取指针指向的参数的值，并一定指针到下一个参数
```objective-C
@interface VarArgs : NSObject
- (void)test:(NSString *) name ,...;
@end

@implementation VarArgs
- (void)test:(NSString *) name ,...
{
	va_list argList;
	if(name)
	{
		va_start(argList,name);
		NSString* arg = va_arg(argList,id);
		while(arg){
			NSLog(@"%@",arg);
			arg = va_(argList,id);
		}
		va_end(argList);
	}
}
@end
```

## 类变量
Java中类定义时，使用static限定的变量，就是类变量
Objective-C不支持真正意义上的泪变量，一般通过内部全局变量来模拟变量
```objective-C
static NSString* nation = nil;
+(NSString*) nation
{
	return nation;
}
+(void)setNation:(NSString*)newNation
{
	if(![nation isEqualToString: newNation])
	{
		nation = newNation;
	}	
}
```

## 单例模式
如果一个类只能创建一个类型，则这个类被称为单例类
在Java中使用private隐藏构造方法，只暴露`getInstance()`方法来获取对象
```java
public class Singletion {
	private static Singleton uniqueInstance = null;

	private Singletion() {

	}

	public static Singleton getInstance() {
		if(uniqueInstance == null) {
			uniqueInstance = new Singleton();
		}
		return uniqueInstance;
	}
}
```
Objective-C通过static全局变量定义单例指针
```objective-C
@interface Singleton : NSObject
+(id) instance;
@end

// .m
static id instance = nil;
@implementation Singleton
+ (id) instance
{
	if(!instance)
	{
		instance = [[super alloc] init];
	}
	return instance;
}
```