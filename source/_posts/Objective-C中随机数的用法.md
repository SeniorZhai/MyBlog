title: Objective-C中随机数的用法
date: 2014-03-20 12:55:07
categories: iOS
tags: [iOS]
---
1. arcrandom()
```Objective-C
// 获取1到x之间的整数的代码如下:
int value = (arc4random() % x) + 1; 
```
2. random()
```Objective-C
// 需要初始化时设置种子
srandom((unsigned int)time(time_t *)NULL); //初始化时，设置下种子就好了。
```
3. CCRANDOM_0_1()
```Objective-C
// cocos2d中使用 ，范围是[0,1]
// [0,5]   CCRANDOM_0_1() 取值范围是[0,1]
float random = CCRANDOM_0_1() * 5; 
```
4. arc4random_uniform
- 推介使用
```Objective-C
// 返回一个小于number的均匀分布的整数
arc4random_uniform(number)
```
还存在其他一些arc4随机函数，详情查看[](https://developer.apple.com/library/mac/documentation/Darwin/Reference/Manpages/man3/arc4random_uniform.3.html)