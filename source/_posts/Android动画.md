title: Android动画
date: 2014-08-29 11:30:11
categories: Android
tags: [动画,Animation]
---
Android 3.0之后的动画框架
<!--more-->
##分类
- Property Animation 属性动画 
- View Animation
    + Tween Animation 补间动画
    + Frame Animation 帧动画
###Property Animation
属性动画设定了规定时间内修改对象的属性，比如背景色和alpha值等。
可以用xml定义，存放路径为：res/animator/filename.xml
可以通过资源的形式引用：R.animator.filename(in java)、@[package:]animator/file
常用的java类包括：ValueAnimator,ObjectAnimator,AnimatorSet
```xml
<!-- together为同时播放 sequenttially为按顺序播放 -->
<set android:ordering=["together"|"sequentially"]>
    <objectAnimator
        android:propertyName="String"
        android:duration="int"
        android:valueFrom="float|int|color"
        android:valueTo="float|int|color"
        android:startOffset="int"
        android:repeatCount="int"
        android:repeatMode=["repeat"|"reverse"]
        android:valueType=["intType"|"floatType"]/>
    <animator
        android:duration="int"
        android:valueFrom="float|int|color"
        android:valueTo="float|int|color"
        android:startOffset="int"
        android:repeatCount="int"
        android:repeatMode=["repeat"|"reverse"]
        android:valueType=["intType"|"floatType"]/>
</set>
```
使用时
```java
AnimatorSet set = (AnimatorSet)AnimatorInflater.loadAnimator(myContext,R.anim.property_animator);
set.setTarget(myObject);
set.start();
```
###View Animation
View Animation包含了Tween Animation、Freme Animation
####Tween Animation
存放路径为res/anim/filename.xml
引用：R.anim.filename(in java)、@[package:]anim/file
补间动画可以对View实现一系列的转换，比如：移动、渐变、伸缩、旋转
Tween Animation只能作用于View对象，只支持一部分属性，比如不支持背景颜色的改变。而且并不改变View对象本身，只是绘制的属性改变了，例如Button改变了位置，但是点击区域仍然不变。
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="@[package:]anim/interpolator_resouce"
    android:shareInterpolator=["true"|"false"]>
    <alpha
        android:fromAlpha="float"
        android:toAlpha="float"/>
    <scale
        android:formXScale="float"
        android:toXScale="float"
        android:formYScale="float"
        android:toYScale="float"
        android:pivotX="float"
        android:pivotY="float"/>
    <translate
        android:fromXDelta="float"
        android:toXDelta="float"
        android:fromYDelta="float"
        android:toYDelta="float"/>
    <rotate
        android:fromDegress="float"
        android:toDegress="float"
        android:pivotX="float"
        android:pivotY="float"/>
</set>
```
使用
```java
ImageView image = (ImageView)findViewById(R.id.image);
Animation hyperspcaJump = AnimationUtils.loadAnimation(this,R.anim.hyperspace_jump);
image.startAnimation(hyperspaceJump);
```
####Frame animation
帧动画是一系列的图片按顺序显示
文件路径res/drawable/filename.xml
引用：R.drawable.filename(in java)、@[package:]drawable/file
```xml
<?xml version="1.0" encoding="utf-8"?>  
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot=["true" | "false"] >  
    <item android:drawable="@[package:]drawable/drawable_resource_name"
    android:duration="integer" />
</animation-list>  
```
使用
```java
ImageView rocketImage = (ImageView) findViewById(R.id.rocket_image);  
rocketImage.setBackgroundResource(R.drawable.rocket_thrust);  
  
rocketAnimation = (AnimationDrawable) rocketImage.getBackground();  
```
rocketAnimation.start(); 
```
注意点：start()不能再onCreat()中调用