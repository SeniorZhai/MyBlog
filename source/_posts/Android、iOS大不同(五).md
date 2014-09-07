title: Android、iOS大不同(五)
date: 2014-09-05 23:01:31
categories: iOS
tags: [Android,iOS,Java,Objective-C,大不同]
---
Objective-C特殊的键指编码(KOC)与监值兼听(KVO)
<!--more-->
##键值编码(KVC)
简直编码用来简化设置对象的属性的方法
最基本的KVC由NSKeyValueCode协议提供，最基本的操作属性的方法如下
- `setValue:value forKey:key` 通过属性名来设置值
- `valueForKey` 通过属性名来获取属性值
在KVC中，调用以上方法存取属性值时，底层的机制如下：
1. 优先使用setter、getter方法实现
2. 不能实现，则搜索`_value`属性
3. 不存在，则搜索`value`属性
4. 都没找到，执行对象的setValue:forUndefinedKey:方法，或者valueforUndefinedKey:方法
> 默认的setValue:forUndefineKey:方法和valueforUndefinedKey:方法会引发一个异常，这个异常会导致程序因为异常结束
```objective-c
@interface Person : NSObject
{
	NSString* name;
	NSDate* birth;
	int age;
}

//main
Person person = [[Person alloc] init];Í
[person setValue:@"Zoe" forKey:@"name"];
[person setValue:1 forKey:@"age"];
[person setValue:[[NSDate alloc] init] forKey:@"birth"];
```
类中存在复合属性，就必须使用Key路径找到属性
- setValue: forKeyPath: 根据Key路径设置属性值
- valueForKeyPath: 根据Key路径获取属性值
```objective-c
@interface Order : NSObject
@property(nonatomic,strong) Item* item;
@property(nonatomic,strong) NSString* amout;
@end

//
Order* order = [[Order alloc] init];
[order setValue:[[Item alloc] init] forKey:@"item"];
[order setValue:@"Zoe" forKeyPath:@"item.name"];
```
##键值监听(KVO)
键值监听(Key Value Observing)机制由`NSKeyValueObserving`协议提供支持，NSObject遵守了该协议。
该协议包含如下常用方法可用于注册静听器：
- addObserver:forKeyPath:optioncontext: 注册一个监听器用于监听指定的key路径
- removeObserver:forKeyPath: 为key路径删除指定的监听器
- removeObserver:forKeyPath:context: 为key路径删除指定的监听器，只是多一个context参数
在MVC模型中，很容易想到让View来监听数据Model的改变，作为监听的视图组建需要重写`observeValueForKeyPath:ofObject:context:`方法，还方法可以得到最新修改的数据。
KVO编程的步骤应该为：
1. 为被监听对象注册监听器
2. 重写监听器的observeValueForKeyPath:ofObject:change:context:方法
```objective-c
@interface ItemView : NSObject
@property(nonatomic,weak) Item* item;
@end

//
@implementation ItemView
@synthesize item = _item;
- (void)setItem:(Item *)item 
{
	self->_item = item;
	[self.item addObserver:self forKeyPath:@"name" option:NSKeyValueObservingOptionNew context:nil];
	[self.item addObserver:self forKeyPath:@"price" option:NSKeyValueObservingOptionNew context:nil];
}
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object chang:(NSDictionary *)change context:(void *)context
{
	NSLog(@"被修改的KeyPath%@",keypath);
	NSLog(@"被修改的对象为%@",object);
	NSLog(@"被修改的属性值%@",[chang objectForKey:@"new"]);
	NSLog(@"被修改的上下文为%@",context);
}
- (void)dealloc
{
	[self.item removeObserver:self forKeyPath:@"name"];
	[self.item removeObserver:self forKeyPath:@"price"];
}
```
