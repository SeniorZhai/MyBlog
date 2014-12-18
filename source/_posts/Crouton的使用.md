title: Crouton的使用
date: 2014-12-15 18:14:10
categories: Android
tags: [Crouton,提示]
---
`Crouton`是一个显示提示信息的显示工具类，可以用来代替Toast。默认显示在窗口的顶部，可以按队列一个接着一个显示。
<!--more-->

##使用
- 通过CharSequence创建Crouton
```java
Crouton.makeText(Activity,CharSequence,Style).show();
```
- 通过字符串资源创建Crouton
```java
Crouton.makeText(Activity,int,Style).show();
```
- 在ViewGroup下附加Crouton
```java
Crouton.makeText(Activity,int,Style,int).show();
Crouton.makeText(Activity,int,Style,ViewGroup).show();
```
- 取消显示
```java
// 可以在onDestroy()函数中添加下面的方法，取消所有预定的Crouton
Crouton.cancelAllCroutons();
```

##默认Style
- Style.ALERT
- Style.CONFIRM
- Style.INFO

###自定义的Style可以设置：
- 显示时间 Configuration.Builder().setDuration()
- 尺寸 
- 显示的文本	
- 自定义视图
- 出现和消失的动画 Configuration.Builder().setInAnimation()/setOutAnimation
- 显示的图像 
- 背景色 Style.Builder().setBackgroundColor()

##导入方式
```
dependencies {
  ...
  compile('de.keyboardsurfer.android.widget:crouton:1.8.5@aar') {
    // exclusion is not neccessary, but generally a good idea.
    exclude group: 'com.google.android', module: 'support-v4'
  }
  ...
}
```

- 例子 <https://github.com/SeniorZhai/CroutonDemo>