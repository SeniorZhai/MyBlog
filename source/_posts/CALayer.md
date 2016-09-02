title: CALayer
date: 2016-06-25 20:29:25
categories: iOS
tags: [view,动画]
---
在iOS当中，所有的视图都从`UIView`的基类派生二来，UIView  可以处理触摸时间，可以支持基于`Core Graphics绘图`，可以做旋转或者缩放。
<!--more-->
## CALayer
`CALayer`类在概念上和`UIView`类似，同样也是一些被层级关系树管理的矩形块，同样也可以包含一些内容，管理子图层的位置。和`UIView`不同的是`CALayer`不处理用户交互。
每一个`UIView`都有一个`CALayer`实例的图层属性，也就是所谓的`backing layer`，视图的职责就是创建并管理这个图层。
UIView没有暴露出一些CALayer的功能：
- 阴影、圆角、带颜色的边框
- 3D变换
- 非矩形范围
- 透明遮罩
- 多级非线性动画

```swift
override func viewDidLoad() {
  super.viewDidLoad()
  contentView.backgroundColor = UIColor.grayColor()
  let layer = CALayer()
  layer.frame = CGRectMake(50, 50, 100, 100)
  layer.backgroundColor = UIColor.blueColor().CGColor
  contentView.layer.addSublayer(layer)
}
```
一个视图只有一个相关联的图层(自动创建)，同时也可以支持添加无数个子图层，并且把它直接添加视图关联图层的子图层

使用图层关联的视图而不是CALayer的好处在于，能在使用所有CALayer底层特性的同时，也可以使用UIView的高级API
当满足以下条件的时候，使用CALayer更为合适
- 开发同时在Mac OS上运行的跨平台应用
- 使用多种CALayer的子类，并且不想创建额外的UIView去包装它们所有
- 做一些对性能特别挑剔的工作，比如对UIView一些可忽略不计的操作都会引起显著的不同


## contents属性
CALayer有一个属性`contents`，这个属性的类型是id，意味着它可以是任何类的对象，给contents赋任何值，都可以编译通过，但是contents不是CGImage，那么得到的图层将是空白。
```swift
  override func viewDidLoad() {
       super.viewDidLoad()
       contentView.backgroundColor = UIColor.grayColor()
       let layer = CALayer()
       layer.frame = CGRectMake(50, 50, 100, 100)

       layer.contents = UIImage(named: "icon")?.CGImage

       contentView.layer.addSublayer(layer)
   }
```
简单的添加就可以显示在UIView中显示图片
### contentGravity
和UIView的contentMode一样，contentGravity可以指定内容在同层边界中对齐方式
- kCAGravityCenter
- kCAGravityTop
- kCAGravityBottom
- kCAGravityLeft
- kCAGravityRight
- kCAGravityTopLeft
- kCAGravityTopRight
- kCAGravityBottomLeft
- kCAGravityBottomRight
- kCAGravityResize
- kCAGravityResizeAspect
- kCAGravityResizeAspectFill
### contentsScale
`contentsScale`属性定义了寄宿图的像素尺寸和视图大小的比例，默认情况下它是一个值为1.0的浮点数
如果`contentsScale`设置为1.0，将会每个点1一个像素绘制1个像素绘制图片，如果设置为2.0，则会每两个点像素绘制图片
### maskToBounds
UIView有一个`clipsToBounds`的属性用来指定是否显示超出边界的内容，CALayer对应的属性叫做`maskToBounds`
### contentsCenter
contentsCenter定义了一个固定的边框和一个图层上可拉伸的区域。改变contentsCenter的值并不会影响到寄宿图的显示。
默认情况下，contentsCenter是{0,0,1,1}，这意味着如果大小改变了，那么寄宿图将会均匀地拉伸开。

## 自定义绘制
给`contents`赋CGImage并不是唯一设置寄宿图的方法，也可以直接用`Core Graphics`直接绘制寄宿图，也能够继承UIView并实现`drawRect`方法来绘制
当视图在屏幕上显示的时候，drawRect方法会被调用，在方法里可以使用Graphics去绘制一个寄宿图。然后内容就会被缓存起来直到它需要更新（通常因为开发者调用了setNeedsDisplay方法）
CALayer有一个可选的delegate属性，实现了CALayerDelegate协议，当CALayer需要一个内容 特定的信息时，就会从协议中请求`drawLayer`方法，CALayerDelegate是非正式协议
```swift
override func viewDidLoad() {
    super.viewDidLoad()
    contentView.layer.delegate = self
    contentView.layer.display()
}

override func drawLayer(layer: CALayer, inContext ctx: CGContext) {
    CGContextSetLineWidth(ctx, 10)
    CGContextSetStrokeColorWithColor(ctx, UIColor.redColor().CGColor)
    CGContextStrokeEllipseInRect(ctx, layer.bounds)
}
```
