title: JumpingBeans
date: 2014-09-11 12:38:59
categories: Android
tags: [TextView,自定义控件]
---
一个可爱向的自定义控件
<!--more-->

##使用
- 将[JumpingBeans](https://gist.github.com/SeniorZhai/ae1338d6d13c5870d913)导入项目，因为使用了`ValueAnimator`，所以默认使用Android Api 11+。也已可以使用[NineOldAndroids](https://github.com/JakeWharton/NineOldAndroids)
- 设置TextView动画
```java
// Append jumping dots
final TextView textView1 = (TextView) findViewById(R.id.jumping_text_1);
jumpingBeans1 = new JumpingBeans.Builder()
        .appendJumpingDots(textView1)
        .build();

// Make the first word's letters jump
final TextView textView2 = (TextView) findViewById(R.id.jumping_text_2);
jumpingBeans2 = new JumpingBeans.Builder()
        .makeTextJump(textView2, 0, textView2.getText().toString().indexOf(' '))
        .build();
```
##其他Api
- setIsWave(false) 可以使跳动对象整个一起跳动
- setLoopDuration(int) 可以设置动画的时间
- setWavePerCharDaley(int) 单个动画之间的延迟
- setAnimatedDutyCycle(float) 动画之间的暂停时间