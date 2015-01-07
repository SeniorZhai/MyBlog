title: "[转]Storyboard中的UIScrollView使用自动布局，使其能够滚动"
date: 2015-01-05 09:36:16
categories: iOS
tags: [UIScrollView]
---
在使用storyboard和xib时，我们经常要用到ScrollView，还有自动布局AutoLayout，但是ScrollView和AutoLayout 结合使用，相对来说有点复杂。根据实践，我说一下我的理解，在故事板或xib中，ScrollView是根据其下面的一个View的大小来确定ContentSize的大小。
<!--more-->
看一下效果
![](http://img.blog.csdn.net/20141212162205588)
1. 创建一个项目，拖拽一个ScrollView到故事板中，如下图
![](http://img.blog.csdn.net/20141212162559687)
2. 选中ScrollView，添加约束
![](http://img.blog.csdn.net/20141212162817609)
3. 拖拽一个View到ScrollView上， 然后添加上下左右四周约束
![](http://img.blog.csdn.net/20141212163323097)
4. 添加完之后， 可能会报一个错， 如下图， 这个暂时别去管
![](http://img.blog.csdn.net/20141212163603637)
5. 我们先确定一下， 我们是需要水平方向的滚动还是竖直方向的滚动，或者水平方向和竖直方向都需要滚动
	a.水平方向和竖直方向都需要滚动的话， 不用添加
	b.水平方向滚动需要添加下面一个约束
![](http://img.blog.csdn.net/20141212164214496)
	c.竖直方向需要添加下面一个约束
![](http://img.blog.csdn.net/20141212164251203)
6. 我们以水平方向滚动为例，我们需要确定我们想要的宽度， 添加一个固定的宽度的约束
![](http://img.blog.csdn.net/20141212164909223)
7. 选中View， 更新一下Frame
![](http://img.blog.csdn.net/20141212165408322)
8. 如果是想要动态设置ScrollView的宽度，也就是设置View的宽度约束的值， 我们将其拉成属性， 然后修改其值
![](http://img.blog.csdn.net/20141212170135013)
9. 如果是确定的宽度， 可以在- (void)updateViewConstraints这个方法中修改，也可以在别处修改
![](http://img.blog.csdn.net/20141212170330999)
10. 现在运行，就可以水平滚动了。 竖直方向的滚动和水平方向滚动的设置差不多。 我们来添加两个View， 先拖拽一个View（我设为灰色）到视图上， 然后添加约束， 如下图
![](http://img.blog.csdn.net/20141212171704166)
11. 再拖拽一个View， 背景颜色设为红色，设置好之后， 将frame设置到我们需要的， 我这边将X设置到600
![](http://img.blog.csdn.net/20141212172104360)
12. 我们给第二个View添加约束，如下图
![](http://img.blog.csdn.net/20141212172416927)
13. 我们还需要设置一个约束， 就是第二个View距离SuperView的距离，就是第二个View的Leading约束
![](http://img.blog.csdn.net/20141212172619933)
14. 然后将这个约束Leading拉成属性，在- (void)updateViewConstraints设置他的值
![](http://img.blog.csdn.net/20141212172931345)
如下图
![](http://img.blog.csdn.net/20141212173250324)
这样子就OK了。 
自动布局需要自己去多多实践， 有很多细节需要注意的。 
这个例子的demo地址:<http://download.csdn.net/detail/h1101723183/8253159>
竖直方向的Demo下载地址:<http://download.csdn.net/detail/h1101723183/8266503>

出自<http://blog.csdn.net/h1101723183/article/details/41895479>