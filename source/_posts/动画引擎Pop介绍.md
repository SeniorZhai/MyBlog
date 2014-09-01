title: 动画引擎Pop介绍
date: 2014-05-04 13:08:08
categories: iOS
tags: [iOS]
---
几天前Facebook开源了旗下应用[Paper](https://code.facebook.com/posts/656530327776932/building-paper/)所使用的动画引擎Pop。使用动态动画(dynamic animations)而不是传统的静态动画(static animations)，Pop使常见的滑动(scrolling)、弹跳(bouncing)、折叠(unfolding)等效果充满了活力，也使Paper给人耳目一新的感觉。

##为什么需要Pop

从第一代iPhone开始，iOS系统在对静态动画(static animations)方面的支持就很出色。苹果提供的Core Animation framework，使我们方便的使用线性动画(linear)、淡入效果(ease-in)、淡出效果(ease-out)等动画。

![](https://github.com/zt1991616/blog/raw/master/Image/14050401.png)

手势交互的革新迎来了新的一轮软件设计的浪潮。人们可以用手指直接操作屏幕上的元素，(而不是想以前，需要一支笔)，这就降低了交互的间接性（反着说，就是交互更直接），于是，人们又提出更进一步的要求：既然触屏可以得到处理，那么不同速度的划屏操作也应该得到处理。

当我们在2010年连个创办Push Pop Press公司的时候，我们的目标就是创造一种可行的、基于物理效果(physics-everywhere)的体验。我们在寻求一种可以使人们非常愉悦、轻松的使用整个应用的解决方案，就像我们使用UIScrollView那样的顺滑。

Popular就是这种理念的最新表现，它使你可以使用你熟悉而且强大的Core Animation进行编程，并且它能够捕获手势操作的速度，更好的反应用户的意图。Paper给我们了一个重新定义这种理念和其背后的动画引擎的机会。

##目标

Pop在三个不同的纬度提供了相应的工具。第一，我们想使一些常见动画在使用上更加的简单便利，除了iOS内建的4种静态动画(static animations),Pop增加三种原始的动画类型：弹簧效果(spring)，衰变效果(decay)和自定义效果(custom)。

![](https://github.com/zt1991616/blog/raw/master/Image/14050402.png)

弹簧效果和衰变效果是动态动画，正是它们让Paper充满了活力动感。弹簧动画使Paper上的元素优雅的弹跳(attractive bounce)。衰变动画可以使元素的移动(movement)缓慢的停止。这两种动画都是把速度作为输入，并且在处理用户手势时的理想方案之一。

第二，Pop是一个可灵活扩展的库。自定义动画(custom animation)允许开发者插自己的动画代码，使你可以容易的创造出独特的动画效果。通过把动画从展示层(display)解耦，Pop是一个适用范围更广的库，你可以对任何对象属性做动态的改变，就像做动画那样。

第三，我们的目标是构建一个开发者有好但是功能强大的编程模型。比如，Core Animation开发者应该对下面的API比较熟悉：
```objective-c
@interface CALayer
/* Attach an animation to the layer. */
- (void)addAnimation:(CAAnimation *)anim forKey:(NSString *)key;

/* Remove all animation attached to the layer. */
- (void)removeAllAnimations;

/* Remove any animation attached to the layer for 'key'. */
- (void)removeAnimationForKey:(NSString *)key;

/* Returns an array containing the keys of all animations currently attached to the receiver. */
- (NSArray *)animationKeys;

/* Returns the animation added to the layer with identifier 'key', or nil if no such animation exists. */
- (CAAnimation *)animationForKey:(NSString *)key;

@end
```

开发者可以通过这些接口，使用Core Animation来开始和结束动画。最显著的一点是，它也支持查询正在中的动画，这是终端动画和构建流畅的用户界面的关键。下面是Popular实现相同功能提供的API：
```objective-c
@interface NSObject (pop)

/* Attach an animation to the layer. */
- (void)pop_addAnimation:(POPAnimation *)anim forKey:(NSString *)kay;

/* Remove all animations attached to the layer. */
- (void)pop_removeAllAnimations;

/* Remove any animation attached to the layer for 'key'. */
- (void)pop_removeAnimationForKey:(NSString *)key;

/* Returns an array containing the keys of all animations currently attached to the receiver. */
- (NSArray *)pop_animationKeys;

/* Returns the animation added to the layer with identifier 'key', or nil if no such animation exists. */
- (POPAnimation *)pop_animationForKey:(NSString *)key;

@end
```

最基本的动画类型是POPAnimation，由于Pop是在NSObject上扩展的类别，所以可以在任何对象上做动画。除了上面提到的区别外，这些API接口形式在其他方面都没有什么区别。

下面的例子展示了如何对layer的bounds属性做弹簧动画：
```objective-c
POPSpringAnimation *anim = [POPSpringAnimation animation];
anim.property = [POPAnimatableProperty propertyWithName:kPOPLayerBounds];
anim.toValue = [NSValue valueWithCGRect:CGRectMake(0,0,400,400)];
[layer pop_addAnimation:anim forKey:@"size"];
```
如果你会使用Core Animation构建动画，那么你也会用Pop，它们在使用上是相同的。
