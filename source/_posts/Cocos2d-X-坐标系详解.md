title: Cocos2d-X 坐标系详解
date: 2014-03-31 11:32:49
categories: Cocos2d-X
tags: [坐标系]
---
Cocos2d-x坐标系和Open GL坐标系相同，都是起源于笛卡尔坐标系。

##笛卡尔坐标系
笛卡尔坐标系中定义右手系原点在左下角，x向右，y向上，z向外
![](https://github.com/zt1991616/blog/raw/master/Image/14033101.gif)

##屏幕坐标系和Cocos2d坐标系
iOS、Android、Windows Phone在开发应用时使用的是标准屏幕坐标系，原点为屏幕左上角，x向右，y向下。

![](https://github.com/zt1991616/blog/raw/master/Image/14033102.png)

##世界坐标系(World Coordinate)和本地坐标系(Node Local)
世界坐标系也叫绝对坐标系，是游戏开发中建立的概念，因此，“世界”指游戏世界。Cocos2d中的元素是由父子关系的层级结构，我们通过Node的`setPosition`设定元素的位置使用的就是相对与父类节点的本地坐标系而非世界坐标系。

本地坐标系也叫相对坐标系，是和节点相关联的坐标系。每个节点都有独立的坐标系，当节点移动或改变方向时，和该节点关联的坐标系将随之移动或改变方向。

##锚点(Anchor Point)

- Anchor Point的两个参数都在0~1之间。它们表示的并不是像素点，而是乘数因子。(0.5, 0.5)表示Anchor Point位于节点长度乘0.5和宽度乘0.5的地方，即节点的中心

- 在Cocos2d-x中Layer的Anchor Point为默认值(0, 0)，其他Node的默认值为(0.5, 0.5)。
我们用以下代码为例，使用默认Anchor Point值，将红色层放在屏幕左下角，绿色层添加到红色层上：
```C++
auto red = LayerColor::create(Color4B(255,100,100,128),visibleSize.width/2,visibleSize.height/2);
auto green = LayerColor::create(Color4B(100,255,100,128),visibleSize.width/4,visibleSize.height/4);
red->addChild(greeen);
this->addChild(red,0);
```
![](https://github.com/zt1991616/blog/raw/master/Image/14033103.png)

我们用以下代码为例，将红色层的Anchor Point设为中点放在屏幕中央，绿色层添加到红色层上，绿色层锚点为右上角：

注：因为Layer比较特殊，它默认忽略锚点，所以要调用ignoreAnchorPointForPosition()接口来改变锚点，关于ignoreAnchorPointForPosition()接口的使用说明，我们将在后面详细讲解。
```C++
auto red = LayerColor::create(Color4B(255, 100, 100, 128), visibleSize.width/2, visibleSize.height/2);
red->ignoreAnchorPointForPosition(false);
red->setAnchorPoint(Point(0.5, 0.5));
red->setPosition(Point(visibleSize.width/2 + origin.x, visibleSize.height/2 + origin.y));
 
auto green = LayerColor::create(Color4B(100, 255, 100, 128), visibleSize.width/4, visibleSize.height/4);
green->ignoreAnchorPointForPosition(false);
green->setAnchorPoint(Point(1, 1));
red->addChild(green);
 
this->addChild(red, 0);
```
![](https://github.com/zt1991616/blog/raw/master/Image/14033104.png)
##忽略锚点(Ignore Anchor Point)

Ignore Anchor Point全称是ignoreAnchorPointForPosition，作用是将锚点固定在一个地方。

如果设置其值为true，则图片资源的Anchor Pont固定为左下角，否则即为所设置的位置。

我们用以下代码为例，将两个层的ignoreAnchorPointForPosition设为true，并将绿色的层添加到红色的层上：
```C++
auto red = LayerColor::create(Color4B(255, 100, 100, 128), visibleSize.width/2, visibleSize.height/2);
red->ignoreAnchorPointForPosition(true);
red->setPosition(Point(visibleSize.width/2 + origin.x, visibleSize.height/2 + origin.y));
 
auto green = LayerColor::create(Color4B(100, 255, 100, 128), visibleSize.width/4, visibleSize.height/4);
green->ignoreAnchorPointForPosition(true);
 
red->addChild(green);
 
this->addChild(red, 0);
```
![](https://github.com/zt1991616/blog/raw/master/Image/14033105.png)

##VertexZ，PositionZ和zOrder
- VerextZ是OpenGL坐标系中的Z值
- PositionZ是Cocos2d-x坐标系中Z值
- zOrder是Cocos2d-x本地坐标系中Z值
在实际开发中我们只需关注zOrder。

可以通过setPositionZ接口来设置PositionZ。

以下是setPositionZ接口的说明：
```
Sets the 'z' coordinate in the position. It is the OpenGL Z vertex value.
```
即PositionZ的值即为opengl的z值VertexZ。同样节点的PositionZ也是决定了该节点的渲染顺序，值越大，但是与zOrder不同的区别在于，PositionZ是全局渲染顺序即在根节点上的渲染顺序，而zOrder则是局部渲染顺序，即该节点在其父节点上的渲染顺序，与Node的层级有关。用以下事例来说明:
```C++
 	auto red = LayerColor::create(Color4B(255, 100, 100, 255), visibleSize.width/2, visibleSize.height/2);
    red->ignoreAnchorPointForPosition(false);
    red->setPosition(Point(visibleSize.width / 2, visibleSize.height / 2));
 
    auto green = LayerColor::create(Color4B(100, 255, 100, 255), visibleSize.width/4, visibleSize.height/4);
    green->ignoreAnchorPointForPosition(false);
    green->setPosition(Point(visibleSize.width / 2, visibleSize.height / 2 - 100));
    red->setPositionZ(1);
    green->setPositionZ(0);
    this->addChild(red, 0);
    this->addChild(green, 1);
```
![](https://github.com/zt1991616/blog/raw/master/Image/14033106.png)

##触摸点(Touch position)
在处理触摸事件时需要重写以下四个函数：
```C++
virtual bool onTouchBegan(Touch *touch,Event *event);
virtual void onTouchEnded(Touch *touch,Event *event);
virtual void onTouchCancel(Touch *touch,Event *event);
virtual void onTouchMoved(Touch *touch,Event *event);
```
在函数中获取到touch，在设计游戏逻辑时需要用到触摸点在Cocos2d坐标系中的位置，就需要将touch的坐标转换成OpenGL坐标系中的点坐标。

Touch position是屏幕坐标系中的点，OpenGL postion是cocos2d-x用到OpenGL坐标系上的点坐标，通常我们在开发中会使用两个接口getLocation()和getLocationInView()来进行相应坐标转换工作。

在开发中一班使用getLocation()获取触摸点的GL坐标，而getLocationo()内部实现是通过`Director::getInstance()->convertToGL(_point);`返回GL坐标。

此外，关于世界坐标系和本地坐标系的互相转换，在Node中定义了以下四个常用的坐标转换的相关方法。
```C++
// 把基于当前节点的本地坐标系下的坐标转换到世界坐标系中
Point convertToNodeSpace(const Point& worldPoint) const;
// 把世界坐标转换到当前节点的本地坐标系中
Point convertToWorldSpace(const Point& nodePoint) const;
// 基于Anchor Point把基于当前节点的本地坐标系下的坐标转换到
Point convertToNodeSpaceAR(const Point& worldPoint) const;
// 基于Anchor Point把世界坐标转换到当前节点的本地坐标系中
Point convertToWorldSpaceAr(const Point& nodePoint) const;
```
例子：
```C++
auto *sprite1 = Sprite::create("HelloWorld.png");
sprite1->setPosistion(ccp(20,40));
sprite1->setAnchorPoint(ccp(0,0));
this->addChild(sprite1);// 此时添加到的是世界坐标系，也就是OpenGL坐标系

auto *sprite2 = Sprite::create("HelloWorld");
sprite2->setPosition(ccp(-5,-20));
sprite2->setAnchorPoint(ccp(1,1));
this->addChild(sprite2);// 此时添加到的是世界坐标系，也就是OpenGL坐标系

//将sprite2这个节点的坐标ccp(5,-20)转换成sprite1节点下的本地坐标系统的位置坐标
Point point1 = sprite1->convertToNodeSpace(sprite1->getPosition());
Point point2 = sprite->convertToWorldSpce(sprite2->getPosition());
// 将sprite2这个节点的坐标ccp(5,-20)转换成sprite1节点下的世界坐标系统的位置坐标
log("postion = (%f,%f)",point1.x,point1.y);
log("postion = (%f,%f)",point2.x,point2.y);
```
![](https://github.com/zt1991616/blog/raw/master/Image/14033107.png)
![](https://github.com/zt1991616/blog/raw/master/Image/14033108.png)

其中:Point point1 = sprite1->convertToNodeSpace(sprite2->getPosition());

相当于sprite2这个节点添加到（实际没有添加，只是这样理解）sprite1这个节点上，那么就需要使用sprite1这个节点的节点坐标系统，这个节点的节点坐标系统的原点在（20，40），而sprite1的坐标是（-5，-20），那么经过变换之后，sprite1的坐标就是（-25，-60）。

其中:Point point2 = sprite1->convertToWorldSpace(sprite2->getPosition());

此时的变换是将sprite2的坐标转换到sprite1的世界坐标系下，而其中世界坐标系是没有变化的，始终都是和OpenGL等同，只不过sprite2在变换的时候将sprite1作为了”参照“而已。所以变换之后sprite2的坐标为:（15，20）。

![](https://github.com/zt1991616/blog/raw/master/Image/14033109.png)