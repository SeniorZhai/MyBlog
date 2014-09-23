title: Android、iOS大不同(三)
date: 2014-09-03 21:54:57
categories: iOS
tags: [Android,iOS,Java,Objective-C,大不同]
---
隐藏和封装
<!--more-->
封装是面向对象的三大特征(另两个是多态和继承)，封装指的是将对象的状态信息隐藏在对象内部，不允许外部程序直接访问对象内部的信息，而是通过该类所提供的方法来实现对内部信息的操作和访问。其目的是：
- 隐藏类的实现细节
- 使用时只能通过事先预定的方法来访问数据，限制不合理的访问，符合设计逻辑
- 进行数据监测，保证对象信息的完整性
- 便于修改提高可维护性
##访问控制符
Objective-C提供了4个访问控制符`@private`、`@package`、`@protected`、`@public`
- @private(当前类访问权限) 只能在当前类的内部访问，在类的实现部分定义的成员变量默认使用这个访问权限
- @oackage(与映像访问权限) 可以在当前类以及当前类实现的同一个映像的任意地方访问
- @protected(子类访问权限) 可以在当前类和当前类的子类的任意地方访问，在类的接口部分定义的成员变量默认使用这种访问权限
- @public(公共访问权限) 可以在任意地方访问
```objective
@interface Person : NSObejct
{
	@private
	NSString* _name;
	int _age;
}
- (void)setName:(NSString *)name;
- (NSString *)getName;
- (void)setAge:(int)age;
- (int)getAge;
@end

@implementation Person
- (void)setName:(NSString *)name
{
	if([name length] > 6 || [name length] < 2)
	{
		return;
	}
	else
	{
		_name = name;
	}
}
- (NSSting *)getName
{
	return _name;
}
- (void) setAge:(int)age
{
	if(age != _age)
	{
		if(age > 100 || age < 0)
		{
			return;
		}
		else
		{
			_age = age;
		}
	}
}
- (int) getAge
{
	return _age;
}
@end
```
Java中也提供了4个访问控制符`private`、`default`、`protected`、`public`
- private 提供了类内部的访问权限
- dufault 是默认的权限，同一包中和类内部可以访问
- protected 提供了同类，同包和子类的访问权限
- public 提供了外部访问权限，即可再任意地方访问
```java
public class Person{
	private String name;
	private int age;

	public void setName(String name){
		if(name.length() >6 || name.lenth()<2){
			return;
		}else{
			this.name = name;
		}
	}
	public String getName(){
		return this.name;
	}
	public void setAge(int age){
		if(age > 100 || age < 0){
			return;
		}else{
			this.age = age;
		}
	}
	public getAge(){
		return age;
	}
}
```
隐藏的目的是为了封装类，达到程序的模块化，实现高内聚(功能实现细节在模块内部完成，不允许外部干预)，低耦合(尽量少地暴露方法给外部使用)
