title: SystemUI与FitsSystemWindows
date: 2016-01-20 12:30:17
categories:
tags:
---
在亘古时代(Android 2.+)的时候，全屏操作简单粗暴，但是切换显示、隐藏时丑陋之极。
<!--more-->
- 在theme中设置
```xml
<application
  android:theme="@android:style/Theme.Holo.NaoActionBar.Fullscreen"
...
</application>
```
- 在activity渲染之前
```java
...
  @Override
  protected void onCreate(Bundle savedInstanceState){
    super.onCreate(savedInstanceState);
    if (Build.VERSION.SDK_INT < 16) {
      getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,WindowManager.LayoutParams.FLAG_FULLSCREEN);
    }
    setContentView(...);
    ...
  }
...
```
因为设置了WindowManager的flag，想要切换只能重置flag，而且因为大小变了，activity的界面也会改变。

在改革春风吹满地的新时代(Android 4.0+)，SDK提供了我们新的选择，使用setSystemUiVisibility()来操控SystemUI，这里不只StatusBar还有NavigationBar
## 4.0可使用的方法
```java
View decorView = getWindow().getDecorView();
int uiOptions = View.SYSTEM_UI_FLAG_HIDE_NAVIGATION | View.SYSTEM_UI_FLAG_FULLSCREEN;
decorView.setSystemUiVisibilit(uiOptions);
```
- 触摸屏幕任何位置都会使得导航、状态栏出现，且`SYSTEM_UI_FLAG_HIDE_NAVIGATION`被清除
- 一旦标志位被清除，需要重新设置
- 在不用地方UI FLAG是不同的，所有最好在onReasume()和onWindowFocusChaned()中设置
- 被调用的View显示时才会生效
## 4.1以后
上面的方法设置后，内容还是会因为场景的变化而变化，所以我们需要让我们的内容放在SystemUI的后面
```java
View decorView = getWindow().getDecorView();
int uiOptions = View.SYSTEM_UI_FLAG_HIDE_NAVIGATION | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION |View.SYSTEM_UI_FLAG_FULLSCREEN | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN;
decorView.setSystemUiVisibilit(uiOptions);
```
值得注意的是，有些控件我们是不希望不覆盖住的，比如toolbar，这个时候需要给它加上FitsSystemWindows属性，保证不会被SystemUI遮住。
从示例上看，系统会为View设置上Padding，所以，如果要做切换时，可能要考虑还原View的Padding。
# 示例
<https://github.com/SeniorZhai/SystemUI>
