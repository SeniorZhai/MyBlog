title: Google IO 第一份惊喜-ConstraintLayout
date: 2016-05-19 14:03:07
categories: Android
tags:[AutoLayout,约束,布局,Layout]
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
在布局编辑窗口中选择`Design`(天呐，我几乎是第一次选择他，而不是直接去使用TextV去写XML)
![](16051904.png)

##约束Constraints
约束可以帮助你设定不同组件之间的位置关系，比如，这个A组件在B组件右边25dp且位于C组件下方8dp的位置。

在布局编辑窗口中，当你选中一个组件可以看到以下情况
![](./img/16051901.png)
拖拽边角的方形，可以控制组件的大小
![](./img/16051902.png)
拖拽四边的圆形，可以设置组件相对四个方向上的距离约束
![](./img/16051903.png)
拖拽下方圆角矩形，可以设置组件间的基线对齐

![](./img/16051905.gif)

https://codelabs.developers.google.com/codelabs/constraint-layout/index.html#5
