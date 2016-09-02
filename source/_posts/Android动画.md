title: Android动画
date: 2014-08-29 11:30:11
categories: Android
tags: [动画,Animation]
---
Android 3.0之后的动画框架
<!--more-->
## 分类
- Property Animation 属性动画 
几乎可以让任何对象动起来，它使一个框架，在一个时间内，使用指定的内插技术来影响任何对象的属性
- View Animation
    + Tween Animation 补间动画 应用于View可以定义一系列位置、大小、旋转和透明度的改变
    + Frame Animation 帧动画 基于单元格的动画，每一帧显示一个不同的Drawable。帧动画可以在一个View中显示，并使用它的Canvas作为投影屏幕

### Property Animation
在Android 3.0(API level 11)引入，通过一个属性动画生成器，在一个给定时间内使用设定的差值算法将属性从一个值转换到另一个值。
属性动画设定了规定时间内修改对象的属性，比如背景色和alpha值等，从简单的View效果，如移动、缩放、View的淡入淡出，到复杂的动画，如运行时的布局改变、曲线变换。
可以用xml定义，存放路径为：res/animator/filename.xml
可以通过资源的形式引用：R.animator.filename(in java)、@[package:]animator/file
常用的java类包括：ValueAnimator,ObjectAnimator,AnimatorSet
- XML文件格式：
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
1. 创建属性动画
最简单的方法为使用ObjectAnimator类，这个类包含有offFloat、ofInt和ofObejct静态方法，可以将目标对象的特定属性在制定的值之间进行转换：
```java
String propertyName = "alpaha";
float from = 1f;
float to 0f;
ObjectAnimator anim = ObjectAnimator.ofFloat(targetObject,propertyName,from,to);
```
> 该对象必须包含getter/setter方法，所以上面实例targetObject必须有getAlpha和setAlpha方法，返回和接受一个浮点型数值
作于与非整数和非浮点数类型的属性时，对象要求提供一个`TypeEvaluator类`的实现，实现evaluate方法以返回一个对象，该对象是当动画为开始对象和结束对象之间动画的制定部分时应该返回的对象
```java
TypeEvaluator<MyClass> evaluator = new TypeEvaluator<MyClass>() {
  public MyClass evaluate(float fraction,MyClass startValue,MyClass endValue){
    MyClass result = new MyClass();
    // 修改心对象，使之代表开始值和结束值之间的给定部分
    return result;
  }  
};
//
ValueAnimator oa = ObjectAnimator.ofObject(evaluator,myClassFromInstance,myClassToInstance);
oa.setTarget(myClassInstance);
oa.start();
```
默认情况下，每个动画只运行300ms并且只运行一次。使用`setDuration`方法改变用来完成一次转换的差值器的总时间，使用`setRepeatCount(ValueAnimator.INFINITE)`制定运行次数，使用`setRepeatMode(ValueAnimator.REVERSE)`设置重复模式。
通过XML设置动画
```xml
<objectAnimator xmlns=android="http://schemas.android.com/apk/res/android"
    android:valueTo="0"
    android:propertyName="alpha"
    android:duration="500"
    android:valueType="floatType"
    android:repeatConut="-1"
    android:repeatMode="reverse"/>
```
通过XML载入动画
```java
AnimatorSet set = (AnimatorSet)AnimatorInflater.loadAnimator(myContext,R.anim.property_animator);
set.setTarget(myObject);
set.start();
```
2. 创建属性动画集
Android包含有AnimatorSet类，用来创建复杂、互相关联的动画
想要向一个动画集中添加一个新的动画，可以使用play方法，这个方法返回一个`AnimatorSet.Builder`对象，通过它可以指定相对于其他动画何时播放指定的动画
```java
AnimatorSet mySet = new AnimatorSet();
mySet.play(firstAnimation).before(concurrentAnim1);
mySet.play(concurent1Anim1).with(concurrentAnim2);
mySet.play(lastAnim).after(concurrentAnim2);
```
3. 使用动画监听器
通过Animator.AnimationListener类可以创建事件处理程序
```java
Animator.AnimatorListener l = new AnimatorListener() {
    public void onAnimationStart(Animator animation){}
    public void onAnimationRepeat(Animator animation){}
    public void onAnimationEnd(Animator animation){}
    public void onAnimationCancel(Animator animation){}
}
anim.addListener(l);
```
##### 插值器
默认情况下，在每个动画开始和结束值之间中所用的差值器是一个非线性的插值器`AccelerateDecelerateInterpolator`提供了开始加速和结束时减速的效果，SDK提供的插值器有：
- AccelerateDecelerateInterpolator 开始和结束时结束变化较慢，在中间的时候加速
- AccelerateInterpolator 开始的时候向后，然后再向前急冲
- AnticipateInterpolator 开始的时候向后，然后再向前急冲一定的值后，最后回到最终的值
- BouceInterpolator 动画结束时弹回
- DecelerateInterpolator 开始时速度变化较快，然后减速
- LinearInterpolator 速度的变化是一个常量
- OvershootInterpolator 开始时向前急冲，超过最终的值，然后再回来
通过`setInterpolator()`方法设置插值器，也可以实现`TimeInterpolator`类来指定一个自定义的差值算法。
### View Animation
View Animation包含了Tween Animation、Freme Animation
#### Tween Animation
- 存放路径：res/anim/filename.xml
- 引用：R.anim.filename(in java)、@[package:]anim/file
- 应用：
	+ Activity间的转换
	+ Activity内布局间的转换
	+ 相同View中不同内容间的转换
	+ 为了用户提供反馈，例如提示进度、通过晃动输入框来说明错误或无效的数据输入
补间动画可以对View实现一系列的转换，比如：移动、渐变、伸缩、旋转
Tween Animation只能作用于View对象，只支持一部分属性，比如不支持背景颜色的改变。而且并不改变View对象本身，只是绘制的属性改变了，例如Button改变了位置，但是点击区域仍然不变。
1. 创建补间动画
补间动画使用`Animation`类来创建，可用的类型有
- AlphaAnimation 可以改变View的透明度
- RotateAnimation 可以在XY平面上旋转选中的View Canvas
- ScaleAnimation 允许缩放选中的View
- TraslateAnimation 在屏幕中移动选中的View(但只能在它原始边界范围内显示)
在xml中的定义格式如下：
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
2. 使用补间动画
通过startAnimation方法可以将动画应用到任意View中，只要传递给这个方法应用的动画或动画集合即可
```java
ImageView image = (ImageView)findViewById(R.id.image);
Animation hyperspcaJump = AnimationUtils.loadAnimation(this,R.anim.hyperspace_jump);
iamge.setRepeatMode(Animation.RESTART);  // 循环 REVERSE为反向运行
image.setRepeatCount(Animaion.INFINITE); // 重复
image.startAnimation(hyperspaceJump);
```
3. 使用动画监听器
AnimationLister可以用于创建一个事件处理程序，当动画开始或结束的时候触发它，监听对象为Animation
```java
myAnimationListener(new AnimationListener(){
	public void onAnimationEnd(Animation animation){
		// 动画执行完成调用
	}
	public void onAnimationStart(Animation animation){
		// 动画开始执行调用
	}
	public void onAnimationRepeat(Animation animation){
		// 在动画重复的时候调用
	}
});
```
4. 为布局和ViewGroup添加动画
LayoutAnimation可以用来为ViewGroup添加动画，并按照预定的顺序把一个动画(或者动画集合)应用到ViewGroup的每一个子View中。
- LayoutAniamationController 可以选择每一个View的开始偏移时间(以毫秒为单位)，以及把动画应用到每一个子View中的顺序和起始时间(正向、反向、随机)
- GridLayoutAnimationController 使用由行和列所映射的网格来向子View分配动画序列
##### 示例：
创建布局动画
```xml
<layoutAnimation
	xmlns:android="http://schemas.android.com/apk/res/android"
	android:delay="0.5"
	android:animationOrder="random"
	android:animation="anim/popin"/>
```
使用布局动画
使用代码或者布局XML资源将其应用到一个ViewGroup中
- 在XML中使用`andorid:layoutAnimation`来完成使用
- 在JAVA代码中使用`setLayoutAnimation`传递动画
```java
	// 载入动画
	Animation set = AnimationUtils.loadAnimation(this, R.anim.pop_in);
	// 创建LayoutAnimation 
	LayoutAnimationController controller = new LayoutAnimationController(
				set);
	// 设置属性
	controller.setOrder(LayoutAnimationController.ORDER_REVERSE);
	//设置控件显示间隔时间；
	controller.setDelay(1);
```
通常情况动画会在ViewGroup第一次进行布局的时候执行一次，可以调用`scheduleLayoutAnimation`来强制动画再次执行，在View Grop下次布局的时候这个动画就会再次执行。
布局动画也支持动画监听
```java
aViewGroup.setLayoutAnimationListener(new AnimationListener() {
	public void onAnimationEnd(Animation _animation){}
	public void onAnimationRepeat(Animation _animation){}
	public void onAnimationStart(Animation _animation){}
});
```
#### Frame animation
帧动画是一系列的图片按顺序显示
- 文件路径res/drawable/filename.xml
- 引用：R.drawable.filename(in java)、@[package:]drawable/file
- xml文件格式
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
rocketAnimation.start(); 
```
注意点：start()不能再onCreat()中调用