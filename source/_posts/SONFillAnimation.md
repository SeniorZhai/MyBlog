title: SONFillAnimation
date: 2014-04-01 13:02:43
categories: iOS
tags: [iOS]
---
`SONFillAnimation`是将一个UIView以指定网格动画显示的类。

##用法
```objective-c
SONFillAnimation *animation = [[SONFillAnimation alloc] initWithNumberOfRows:3 columns:3 animationType:SONFillAnimationTypeDefault];
animation.direction = SONFillAnimationDirectionLeftToRightAndTopDown;
animation.duration = 0.3;
animation.xDelay = 0.1;
animation.yDelay = 0.2;
[animation animateView:view completion:^{}];
```
`numberOfRows`指行数，`numberOfColumns`指列数
`xDelay`指横向网格之间延迟的属性，`yDelay`指纵向网格之间延迟的属性
`anchorPoint`指定每个网格的锚点
`direction`指定顺序方向

您可以指定以下其中一个方向：

- SONFillAnimationDirectionLeftToRightAndTopDown
- SONFillAnimationDirectionLeftToRightAndBottomUp
- SONFillAnimationDirectionRightToLeftAndTopDown
- SONFillAnimationDirectionRightToLeftAndBottomUp
- SONFillAnimationDirectionRandom

`animationType`是一个属性，表示一个类型的动画。

您可以指定在以下的animationType：

- SONFillAnimationTypeDefault
- SONFillAnimationTypeFoldOut
- SONFillAnimationTypeFoldIn
- SONFillAnimationTypeCustom

`setRotationVector:::`是定义为FoldIn/折页动画旋转向量的方法。如果动画类型是自定义的，就不需要设置这个值。默认值是（0，-1,0）。