title: Android Animation
date: 2014-07-21 13:42:57
categories: Android
tags: [Android]
---
Android 3.0(Honeycomb)之前，Android支持两种动画tween animation,frame animation，3.0引入了一个新的动画————property animation。可通过[NineOldAndroids](http://nineoldandroids.com/)实现对低版本的支持。
## View Animation(Tween Animation)补间动画
给出两个关键帧，通过一些算法将给定属性值在给定的时间内在两个关键帧间渐变。View Animation只能应用于View对象，而且只支持一部分属性，如支持缩放旋转不支持背景颜色的改变。
ViewAnimation就是一系列View形状的变换，如大小的缩放、透明度的改变、位置的改变，动画的定义既可以用代码定义也可以使用XML定义。
用XML定义的动画在/res/anim/文件夹内，XML文件的根元素可以是<alpha>,<scale>,<translate>,<rotate>和<interpolator>,<set>。默认情况下，所有动画是同时进行的，可以通过startOffset属性设置哥哥动画的开始偏移量来达到动画顺序播放的效果。可以设置<interpolator>属性改变动画的渐变效果，如AccelerateInterpolator，开始时慢，然后逐渐加快。
```java
Animation hyperspaceJumpAnimation=AnimationUtils.loadAnimation(this, R.anim.hyperspace_jump);
spaceshipImage.startAnimation(hyperspaceJumpAnimation);
```
## Drawable Animation(Frame Animation)
Drawable Animation(Frame Animation)帧动画，通过一系列Drawable依次显示来模拟动画
```xml
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="true">
    <item android:drawable="@drawable/rocket_thrust1" android:duration="200" />
    <item android:drawable="@drawable/rocket_thrust2" android:duration="200" />
    <item android:drawable="@drawable/rocket_thrust3" android:duration="200" />
</animation-list>
```
必须以<animation-list>为根元素，以<item>表示要轮换显示的图片，duration属性表示各项显示的时间，XML文件要放在/res/drawable/目录下。
```java
imageView.setBackgroundResource(R.drawable.drawable_anim);
anim = (AnimationDrawable) imageView.getBackground();

anim.stop()
anim.start()
```
## Property Animation
属性动画
- Duration:动画的持续时间
- TimeInterpolation:属性值的计算方式，比如先快后慢
- TypeEvaluator:根据属性的开始、结束值与TimeInterpolation计算出的因子计算出当前时间的属性值。
- Repeat Count and behavoir:重复次数与方式，比如播放3次、5次、无限循环，可以一直重复或播放完再反向播发
- Animation Sets:动画集合，即可以同时对一个对象应用几个动画，这些动画可以同时播放也可以对不同动画设置不同时间开始偏移
- Frame refreash delay:多少时间刷新一次，即每隔多少时间计算一次属性值，默认是10ms，最终刷新时间还受到系统进程调度与硬件的影响

### Property Animation的工作方式
这面的动画该对象在40ms在x轴移动40pixel。默认10ms刷新一次，这个对象移动了4次，每次移动40/4=10pixel

![](https://github.com/zt1991616/blog/raw/master/Image/14072101.png)
也可以改变属性值的改变方法，设置不同的interpolation，比如下图运动的速度逐渐增大后再逐渐减小

![](https://github.com/zt1991616/blog/raw/master/Image/14072102.png)

下图显示了与上述动画相关的关键对象

![](https://github.com/zt1991616/blog/raw/master/Image/14072103.png)
ValueAnimator表示一个动画，包含动画的开始值、结束值、持续时间等属性
ValueAnimator封装了一个TimeInterpolator，定义了属性在开始值与结束值之间的插值计算方法
ValueAnimator还封装了一个TypeAnimator，根据开始值、结束值与TimeIniterpolator计算得到的值计算出属性值
ValueAnimator根据动画已进行的时间跟动画总时间（duration）的比计算出一个时间因子（0~1），然后根据TimeInterpolator计算出另一个因子，最后TypeAnimator通过这个因子计算出属性值，如上例中10ms时：
首先计算出时间因子，即经过的时间百分比：t=10ms/40ms=0.25
经插值计算(inteplator)后的插值因子:大约为0.15，上述例子中用了AccelerateDecelerateInterpolator，计算公式为（input即为时间因子）：
```java
(Math.cos((input + 1) * Math.PI) / 2.0f) + 0.5f;  
```
最后根据TypeEvaluator计算出在10ms时的属性值：0.15*（40-0）=6pixel。上例中TypeEvaluator为FloatEvaluator，计算方法为 ：
```java
public Float evaluate(float fraction, Number startValue, Number endValue) {
    float startFloat = startValue.floatValue();
    return startFloat + fraction * (endValue.floatValue() - startFloat);
}
```
### ValueAnimator
ValueAnimator包含Property Animation动画的所有核心功能，如动画时间，开始、结束属性值，相应时间属性值计算方法等。应用Property Animation有两个步聚：
1. 计算属性值
2. 根据属性值执行相应的动作，如改变对象的某一属性。

ValuAnimiator只完成了第一步工作，如果要完成第二步，需要实现ValueAnimator.onUpdateListener接口，这个接口只有一个函数onAnimationUpdate()，在这个函数中会传入ValueAnimator对象做为参数，通过这个ValueAnimator对象的getAnimatedValue()函数可以得到当前的属性值如：
```java
ValueAnimator animation = ValueAnimator.ofFloat(0f,1f);
animation.setDuration(1000);
animation.addUpdateListener(new AnimatorUpdateListener(){
    @Override
    public void onAnimationUpdate(ValueAnimator animation) {
        Log.i("update",((Float)animation.getAnimatedValue()).toString());
    }
});
animation.setInterpolator(new CycleInterpolator(3));
animation.start();
```
onAnimationUpdate()通过监听这个事件的值更新时执行的操作，对ValueAnimation一般要监听此事件执行相应的动作，不然则没有意义。ObjectAnimator中会自动更新属性，如无必要不必监听。在函数中会传递一个ValueAnimator参数，通过此参数的getAnimatedValue()取得当前动画属性值。
AnimatorListener用于监听动画的执行过程，有如下一些回调方法
```java
onAnimationStart()
onAnimationEnd()
onAnimationRepeat()
onAnimationCancel()
```
可以继承AnimatorListenerAdapter而不是实现AnimatorListener接口来简化操作，这个类对AnimatorListener中的函数都定义了一个空函数体，这样我们就只用定义想监听的事件而不用实现每个函数却只定义一空函数体。
```java
ObjectAnimator oa = ObjectAnimator.ofFloat(tv,"alpha",0f,1f);
oa.setDuration(3000);
oa.addListener(new AnimatorListenerAdapter(){
    public void on AnimationEnd(Animator animation){
        Log.i("Animation","end");
    }
});
oa.start();
```
### ObjectAnimator
继承自ValueAnimator，要指定一个对象即带对象的一个属性，当属性值计算完成时，自动设置为该对象的相应属性。使用ObjectAnimator，需要满足下面条件
- 对象应该有一个setter函数
- 创建时，使用工厂方法，第一个参数是对象名，第二个为属相名，后面的参数为可变参数，只设置一个值为目的值，属性值的变化范围为当前值到目的值，为了获得当前值，对象必须有相应的getter方法
- getter方法与其setter方法返回类型一致
```java
tv=(TextView)findViewById(R.id.textview1);
btn=(Button)findViewById(R.id.button1);
btn.setOnClickListener(new OnClickListener() {
　　@Override
　　public void onClick(View v) {
　　　　ObjectAnimator oa=ObjectAnimator.ofFloat(tv, "alpha", 0f, 1f);
　　　　oa.setDuration(3000);
　　　　oa.start();
　　}
});
```
View对象属性：
1. translationX和translationY:增量位置，View从它布局容器的左上角坐标开始的位置
2. rotation、rotationX和ratationY:这三个属性控制View对象围绕支点进行2D和3D的旋转。
3. scaleX、scaleY:控制对象围绕其支点进行2D缩放
4. pivotX、pivotY:控制支点的位置，默认为View对象的中心点
5. x、y:控制View对象在它容器中的最终位置
6. alpha:View对象的透明度，默认为1(不偷明)，0代表完全透明
### 通过AnimationSet应用多个动画
AnimationSet提供一个把多个动画组合成一个组合的机制，并可以设置组中动画的时序关系。
```java
// anim1播发完，同时播发anim2、anim3、anim4，之后播发anim5
AnimationSet bouncer = new AnimatorSet();
bouncer.play(anim1).before(anim2);
bouncer.play(anim2).with(anim3);
bouncer.play(anim2).with(anim4);
bouncer.play(anim4).with(anim2);
bouncer.start();
```
### TypeEvalutors
根据属性的开始值、结束值与TimeInterpolation计算出的计算因子计算出当前时间的属性值，Android提供方了以下几个evalutor：
- IntEvaluator:属性的值为int
- FloatEvaluator：属性的值为float
- ArgbEvaluator:属性值为十六进制颜色值
- TypeEvaluator:一个接口，可以通过实现该接口自定义Evaluator
自定义TypeEvalutor很简单，只需要实现一个方法
```java
public class FloatEvaluator implements TypeEvaluator {
    public Object evaluate(float fraction,Object startValue,Object endValue) {
        float startFloat = ((Number)startValue).floatValue();
        return startFloat + fraction * (((Number) endValue).floatValue() - startFloat();
    }
}
```
### TimeInterplator
Time interplator定义了属性值变化的方式，在Property Animation中是TimeInterplator，在View Animation中是Interplator，这两个是一样的，在3.0之前只有Interplator，3.0之后实现代码转移至了TimeInterplator。Interplator继承自TimeInterplator，内部没有任何其他代码。
- AccelerateInterpolator　　　　　     加速，开始时慢中间加速
- DecelerateInterpolator　　　 　　   减速，开始时快然后减速
- AccelerateDecelerateInterolator　   先加速后减速，开始结束时慢，中间加速
- AnticipateInterpolator　　　　　　  反向 ，先向相反方向改变一段再加速播放
- AnticipateOvershootInterpolator　   反向加回弹，先向相反方向改变，再加速播放，会超出目的值然后缓慢移动至目的值
- BounceInterpolator　　　　　　　  跳跃，快到目的值时值会跳跃，如目的值100，后面的值可能依次为85，77，70，80，90，100
- CycleIinterpolator　　　　　　　　 循环，动画循环一定次数，值的改变为一正弦函数：Math.sin(2 * mCycles * Math.PI * input)
- LinearInterpolator　　　　　　　　 线性，线性均匀改变
- OvershottInterpolator　　　　　　  回弹，最后超出目的值然后缓慢改变到目的值
- TimeInterpolator　　　　　　　　   一个接口，允许你自定义interpolator，以上几个都是实现了这个接口
### Layout改变时应用动画
ViewGroup中的子元素可以通过setVisibility使其Visible、Invisible或Gone，当有子元素可见性改变时(VISIBLE、GONE)，可以向其应用动画，通过LayoutTransition类应用此类动画：
```java
transition.setAnimator(LayoutTransition.DISAPPEARING,customDisappearingAnim);
```
通过setAnimator应用动画，第一个参数表示应用环境，第二个参数是一个动画，环境可以是以下4种:
- APPEARING 当一个元素在其父元素中变为Visible时对这个元素应用动画
- CHANGE_APPEARING 当一个元素在其父类中变为Visible时，因系统要重新布局有一些元素需要移动，对这些要移动的元素应用动画
- DISAPPEARING 当一个元素在其父元素中变为GONE时对其应用动画
- CHANGE_DISAPPEARING 当一个元素在其父元素中变为GONE时，因系统要重新布局有一些元素需要移动，这些要移动的元素应用动画
```java
mTransitioner.setStagger(LayoutTransition.CHANGE_APPEARING, 30);
```
此函数设置动画延迟时间，参数分别为类型与时间。
### Keyframes
keyFrame是一个`时间/值`对，通过它可以定义一个在特定时间的特定状态，即关键帧，而且在两个keyFrame之间可以定义不同的Interpolator，就像多个动画的拼接，第一个动画的结束点是第二个动画的开始点，KeyFrame是一个抽象类，要通过ofInt()、ofFloat()、ofObject()获得适当的KeyFrame，然后通过PropertyValuesHolder.ofKeyframe获得PropertyValueHolder对象，如下例子：
```java
Keyframe kf0 = Keyframe.ofInt(0, 400);
Keyframe kf1 = Keyframe.ofInt(0.25f, 200);
Keyframe kf2 = Keyframe.ofInt(0.5f, 400);
Keyframe kf4 = Keyframe.ofInt(0.75f, 100);
Keyframe kf3 = Keyframe.ofInt(1f, 500);
PropertyValuesHolder pvhRotation = PropertyValuesHolder.ofKeyframe("width", kf0, kf1, kf2, kf4, kf3);
ObjectAnimator rotationAnim = ObjectAnimator.ofPropertyValuesHolder(btn2, pvhRotation);
rotationAnim.setDuration(2000);
```
上述代码设置了btn对象的width属性值：
1. 开始动画时，width=400
2. 开始动画1/4时，width=200
3. 开始动画1/2时，width=400
4. 开始动画3/4时，width=100
5. 结束动画时,width=500
第一个参数是时间百分比，第二个参数是在第一个参数时间点的属性值
定义了一些Keyframe后，通过PropertyValuesHolder类的方法ofKeyframe一个PropertyValuesHolder对象，然后通过ObjectAnimator.ofPropertyValuesHolder获得一个Animator对象。
用下面的代码可以实现同样的效果（上述代码时间值是线性，变化均匀）：
```java
ObjectAnimator oa = ObjectAnimator.ofInt(btn2,"width",400,200,400,100,500);
oa.setDuration(2000);
oa.start();
```
### Animating Views
