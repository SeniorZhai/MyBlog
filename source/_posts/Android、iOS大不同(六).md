title: Android、iOS大不同(六)
date: 2014-09-06 09:32:06
categories: iOS
tags: [Android,iOS,Java,Objective-C,大不同]
---
协议或者接口，用于多个类应该遵守的规范。不需要提供实现，不关心内部的状态数据，体现了规范和实现分离的设计哲学。
<!--more-->
##接口
在Java中实现此类设计的方法用接口(interface)
```java
// 接口的定义
[修饰符] interface 接口名 extends 父接口1,父接口2...
{
	零到多个常量定义...
	零到多个抽象方法定义...
}
```
- 修饰符采用public或缺省，即采用包权限或者外部权限
- 接口的命名与类相同
- 一个接口可以有多个直接父类，但接口只能继承接口
>在接口中定义的变量默认会被加上static和final成为静态常量，切所有的常量、方法、内部类、枚举类都是public权限
```java
public interface Output
{
	int MAX_CACHE = 50;
	void out();
	void getDate(String msg);
}
```
##协议
在Objective-C中使用协议(protocol)实现该设计需求
```objective-c
@protocol 协议名 <父协议1,父协议2...>
{
	零到多个方法定义
}
```
- 命名规则与类相同
- 一个协议可以继承多个父协议，但是不能继承类
- 协议定义的方法不能有方法实现，即可以使类方法，也可以使实例方法
```objective-c
@protocol Output
- (void)out;
- (void)getDate(NSString* msg);
```
Obejctive-C可以指定关键字`@optional`、`@required`限定实现类是否必须实现该方法
```objective-c
@protocol Output
@optional
- (void) output;
@required
- (void) getDate (NSString*) msg;
```