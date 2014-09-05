title: Android、iOS大不同(四)
date: 2014-09-04 19:01:55
categories: Android
tags: [Android,iOS,Java,Objective-C,大不同]
---
存取方法，Java没有明确限定Class要由固定的存取方法，但是符合JavaBean格式的类，都应该提供setter、getter存取方法。而Objective-C 2.0开始，自动合成setter、getter方法。
<!--more-->
Java中将成员变量限定为private，提供public的setter、getter存取方法，不再赘述。
```java
public class MyClass {
	private int value;
	public int getValue(){
		rerturn value;
	}
	public void setValue(value){
		this.value = value;
	}
}
```
在OC中，自动合成setter、getter方法分两步完成
1. 在类接口部分使用`@peoperty`指令定义属性
2. 在类的实现部分使用`@synthesize`指令声明该属性
以上步骤不但会合成成对的setter和getter方法，还会自动在类中实现一个与getter方法同名的成员变量。
> Xcode 4.0编码规范推荐成员变量定义以下画线开头，在类实现部分使用`@synthesize property名 [= 成员变量]`指定成员变量名，如果没有指定，成员变量默认和property变量同名。
```objective-c
@interface User : NSObject
@property (nonatomic) NSString* name;
@property NSString* pass;
@property NSDate* birth;
@end
@implementation User
@synthesize name =_name; // 底层默认会实现_name
@synthesize pass; 
@synthesize birth;
// 自定义实现一个setter
- (void)setName:(NSString *)name
{
	self->_name = [NSString stringWithFormat:@"+++%@",name];
}
@end
```
在定义property时，可以增加一些指示符
- assign 简单赋值，不更改所附值的引用计数，适用于NSInteger等基础类，以及short、float、double、结构体等各种C数据类型
- atomic(nonatomic) 是否原子操作，即是否保证线程安全，atomic是默认值
- copy 指定被赋值的对象复制一个副本，将副本值赋给变量
- getter、setter 自定义存取方法名，比如`@property(getter=abc,setter=xyz:)`，注setter方法要带参数不要忘记冒号
- readonly、readwrite 只提供getter或者同时提供getter和setter
- retain 在未开启ARC时，保证引用计数
- strong、weak strong指定被赋值对象持有强引用，weak表示弱引用，只要强引用指向被赋值的对象，对象就不会被自动回收，若引用则不然
- unsafa_unretained 与weak基本相熟，只是unsafa_unretained被回收后不会被赋为nil
只要属性完成了存取方法，OC就可以通过点语法来访问属性，本质上是用setter和getter来访问的
```objective-c
User user = [[User alloc] init];
user.name = @"zoe";
```