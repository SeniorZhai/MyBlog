title: GoogleIO第一份惊喜-ConstraintLayout
date: 2016-05-19 14:03:07
categories: Android
tags: [AutoLayout,约束,布局,Layout]
---
了解iOS开发的童鞋应该知道，自iPhone6推出后，iOS也进入多屏适配时代，AutoLayout成为了适配的首选，Storyboard+AutoLayout成为了iOS布局的主要流派之一（代码适配也是不错的选择）。
<!--more-->
拖拽组件，设定约束，一个界面就基本完成了（当然，还有一堆高级的用法）。而Android开发工程师还在苦逼地将设计稿分割，这部分用一个RelativeLayout，这部分用LinearLayout……然后嵌套一下，或者再套一层。对于很多对几大布局不是很了解的同学，往往几个界面的组成就够头痛一阵了。
拯救你们的救星来了，ConstraintLayout将解决你的难言之隐，还在等待什么，赶快拿起电话*&……%￥(不好意思出戏了)

##开始
首先确保你的Android Studio是[2.2 preview](http://tools.android.com/download/studio/canary)或者更高版本，预览拖拽设定约束需要新的布局编辑窗口，低版本AS应该可以用ConstraintLayout，但没用设置约束功能（光手写XML，为什么还用ConstraintLayout）

ConstraintLayout是一个单独的支持包，所以需要在gradle中添加引用
```
dependencies {
  ...
  compile 'com.android.support.constraint:constraint-layout:1.0.0-alpha'
}
```
在Layout文件中引用
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</android.support.constraint.ConstraintLayout>
```

##使用
![](/img/16051900.jpeg)
在布局编辑窗口中选择`Design`(我几乎是第一次选择他，而不是直接去使用TextV去写XML)
![](/img/16051901.png)

###约束Constraints
约束可以帮助你设定不同组件之间的位置关系，比如，这个A组件在B组件右边25dp且位于C组件下方8dp的位置。

在布局编辑窗口中，当你选中一个组件可以看到以下情况
![](/img/16051902.png)
拖拽边角的方形，可以控制组件的大小
![](/img/16051903.png)
拖拽四边的圆形，可以设置组件相对四个方向上的距离约束
![](/img/16051904.png)
拖拽下方圆角矩形，可以设置组件间的基线对齐

##基本使用
当拖动大小约束时，大小会被改变
![](/img/16051905.gif)
当拖动位置约束时，锚点变绿即建立约束成功
![](/img/16051906.gif)
当约束建立成功后，再次点击锚点即可删除约束
![](/img/16051907.gif)
设置基线约束，可以对齐文本
![](/img/16052001.gif)

当然拖动并不能准确的设置约束，这个时候我们看看到右边的属性窗口
![](/img/16051908.jpeg)
在这里你可以设置组件的相关约束和组件的相关属性
在属性窗口中，你可以看到约束的UI界面是这样的
![](/img/16051909.png)
每个约束是一个`I`型的图标，点击可以切换如下三个状态
![](/img/16051910.png)
Fixed：固定尺寸约束，指定组件大小
![](/img/16051911.png)
AnySize：占用可用空间
![](/img/16051912.png)
Wrap Content：包含组件内容大小

以上可以在[官方示例](https://github.com/googlecodelabs/constraint-layout)中查看

##缺点
- UI操作并不顺畅，控件经常点击不到
- 缺少等宽等约束，复杂的需求不一定能实现
- 当ConstraintLayout不是根布局时， UI操作基本不可用
> 注：以上可能是本人并不熟练或不够了解导致的
