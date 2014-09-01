title: Objective-C块语法
date: 2014-03-08 12:51:07
categories: iOS
tags: [iOS]
---
- 块语法与其他变量类似，不同的是，代码块存储的数据是一个函数体。使用代码块时，可以想调用其他标准函数一样，传入参数，并得到返回值
- 脱字符(^)是块的语法标记，按照参数语法定义返回值以及快主题
![](https://github.com/zt1991616/blog/raw/master/Image/14030801.png)
按照调用函数待方式调用块对象变量就可以了：
int result = myBlock(4)// result = 28
- 例子
1. 参数是NSString*的代码块
```Objective-C
void (^printBlock)(NSString *x)
printBlock = ^(NSString* str)
{
	NSLog(@"print:%@",str);
}
printBlock(@"hello world");
```
2. 代码用在字符串数组排序
```Objective-C
NSArray *stringArray = [NSArray arrayWithObjects:@"abc 1",@"abc2",@"abc 3",@"abc 4",nil];
NSCompartor sortBlock = ^(id string2,id string2)
{
	return [string1 compare:string2];
}
NSArray *sortArray = [stringArray sortedArrayUsingComparator:sortBlock];
NSLog(@"sortArray:%@",sortArray);
```
3. 代码快的递归调用
```Objective-C
static void(^ const blocks)(int) = ^(int)
{
	if(i > 0){
		NSLog(@"num:%d",i);
		blocks(i - 1);
	}	
};
blocks(3);
```
4. 在代码块中使用局部变量和全局变量
```Objective-C
int global = 1000;
int main(int argc,const char * argv[])
{
	@autoreleasepool{
		void(^block)(void) = ^(void)
		{
			global++;
			NSLog(@"global:%d",global);
		}
		block();
		NSLog(@"global:%d",global);
	}
	return 0;
}
```