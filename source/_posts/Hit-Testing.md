title: 图层几何学
date: 2016-06-26 21:10:50
categories: iOS
tags: [布局]
---
UIView有三个比较重要的布局属性：`frame`，`bounds`，`center`，对应到CALayer叫做`frame`，`bounds`，`position`
<!--more-->
- `frame`代表图层的外部坐标，也就是父图层上占据的空间
- `bounds`内部坐标，{0，0}是图层的左上角
- `center`、`position`都代表了相对父图层的`anchorPoint`所在的位置
![](/img/16072501.jpeg)

## 锚点
默认的anchorPoint位于图层的中点，所以图层将会以这个点为中心放置，anchorPoint属性并没有暴露个UIView接口暴露出来，这也就是视图position属性被叫做`center`，但是图层的`anchorPoint`可以被移动
![](/img/16072601.jpeg)
`anchorPoint`用单位坐标来描述，图层左上角是{0,0}，右下角是{1,1}，默认坐标是{0.5,0.5}，`anchorPoint`也可以通过指定x和y值小于或大于1，使它放置在图层范围之外。

## 坐标系
一个图层的`position`依赖于它父图层的`bounds`，如果父图层发生了移动，它的所有子图层也会跟着移动
定义一个图层坐标系下的点或者矩形转换成另一个图层坐标系下的点或者矩形
- `convertPoint(p: CGPoint, toLayer:CALayer)`
- `convertPoint(p: CGPoint,toLayer:CALayer)`
- `convertRect(rect: CGRect,fromLayer: CALayer)`
- `convertRect(rect: CGRect,toLayer: CALayer)`

### 翻转几何结构
通常iOS一个图层的position位于父图层的左上角，Mac OS则位于左下角，Core Animation可以通过`geometryFlipped`属性来适配这两种情况，它决定了一个图层的坐标系是否相对父图层垂直翻转。

### Z坐标轴
CALayer存在一个三维空间，除了`posistion`和`anchorPoint`属性外，CALayer还有另外两个属性，`zPosition`和`anchorPointZ`两者都是在Z轴上描述图层位置的浮点类型。
`zPosition`决定图层显示顺序，图层根据子图层的`sublayers`出现的顺序来类绘制。
```swift
self.greenView.layer.zPosition = 1.0f
```
