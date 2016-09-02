title: Pop动画引擎
date: 2014-12-17 16:08:08
categories: iOS
tags: [iOS,Animation,动画]
---
POP动画极为流畅，秘密就在于POP通过`CADisplayLink`高达60FPS的特性，打造了一个游戏级的动画引擎。
`CADisplayLink`是一个类似`NSTimer`的定时器，不同之处在于，`NSTimer`用于定义任务的执行周期，它的执行收到了CPU阻塞影响，而`CADisplayLink`则用于定义画面的重绘，动画的演变，基于帧(frames)的间隔。通过`CADisplayLink`程序的重绘速度设定到和屏幕刷新频率一致，因此可以得到流畅的交互动画。

## 基本类型
### Spring Animation
Spring Animation提供了一个弹簧效果的动画，通过一系列参数的设置，完成风骚的动画
- Bounciness 反弹，影响动画作用的参数的变化幅度
- Speed 速度
- Tension 拉力，影响回弹力度以及速度
- Friction 摩擦力，如果开启，动画会不断重复，幅度逐渐削弱，知道停止
- Mass 质量，细微的影响动画的回弹力度以及速度

```objective-c
// 创建一个二维平面沿X轴和Y轴进行缩放的动画
POPSpringAnimation *anim = [POPSpringAnimation animationWithPropertyNamed:kPOPLayerScaleXY];
// 指定缩放倍数，不指定fromValue，POP会默认使用当前大小
anim.toValue = [NSValue valueWithCGPoint:CGPointMake(2.0,2.0)];
anim.springBounciness = 4.0;
anim.springSpeed = 12.0;
// 指定Callback，在动画执行的过程中，会不断调用该block
anim.completionBlock = ^(POPAnimation *anim,BOOL finished){
	if(finished) {NSLog(@"animation finished!");}
};
```

## Decay Animation
Decay Animation变现为一个衰减效果的动画，设置一个参数`velocity`(速率)
```objective-c
POPDecayAnimation *anim = [POPDecayAnimation animWithPropertyNamed:kPOPLayerPositionX];
anim.velocity = @(100.0);
anim.fromValue = @(25.0);
anim.completionBlock = ^(POPAnimation *anim,BOOL finished){
	if(finished){NSLog(@"Stop");}	
};
```
设置`deceleration`（负向速度）可以设置一个加速度量

## Property Animation & Basic Animation
Property Animation为属性动画，是Spring Animation和Decay Animation的父类
```objective-c
POPBasicAnimation *anim = [POPBasicAnimation animation];
anim.duration = 10.0;
// 一个慢开慢停的动画
anim.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
// 定义属性
POPAnimatableProperty * prop = [POPAnimatableProperty 
									propertyWithName:@"count" 
									initializer:^(POPMutableAnimatableProperty *prop)
										{ prop.readBlock = ^{id obj,CFFloat values[])
											// 告诉POP如何获取值
											{ value[0] = [[obj description] floatValue];};
										  prop.writeBlock = ^(id obj,const CFFloat values[])
										  	// 如何改变值
										  	{ [obj setText:[NSString stringWithFormat:@"%.2f",values[0]]];};
									// 设置动画变化阀值
									prop.threshold = 0.01;}];
anim.property = prop;
anim.fromValue = @(0.0);
anim.toValue = @(100.0);
```

