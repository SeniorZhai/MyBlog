title: "onSaveInstanceState&onRestoreInstanceState"
date: 2014-10-31 23:17:10
categories: Android
tags: [保存数据,SaveInstance]
---
<!--more-->
##onSaveInstanceState
当Activity可能会被销毁时，该Activity的`onSaveInstanceState`会被执行，除非Activity是主动销毁的。`onSaveInstanceState`的调用遵循一个规则，即未经允许，则onSaveInstanceState会被系统调用，这是一个`保留数据`的机会.
通常`onSaveInstanceState`被调用的情况有：
1. 当用户按下HOME键时
2. 长按HOME键，选择运行其他的程序时
3. 按下电源键（关闭屏幕显示）时
4. 从Activity启动新的Activity时
5. 屏幕切换时
	屏幕切换时，系统会销毁Activity再自动创建Activity，onSaveInstance一定会被执行

##onRestoreInstanceState
onRestoreInstanceState会在Activity被系统销毁后，被重建时调用，在Activity的onStart之后调用
```java
protected void onSaveInstanceState(Bundle savedInstanceState) {
    super.onSaveInstanceState(icicle);
    savedInstanceState.putLong("param", value);
}
public void onCreate(Bundle savedInstanceState) {
    if (savedInstanceState != null){
        value = savedInstanceState.getLong("param");
    }
}
```