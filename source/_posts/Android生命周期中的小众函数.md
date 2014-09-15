title: Android生命周期函数
date: 2014-09-15 14:26:01
categories: Android
tags: [生命周期]
---
<!--more-->
![](/img/14091501.png)
##onContentChanged
不算小众，当Activity的布局改动时，即setContentView()或者addContentView()方法执行完毕时调用该方法，View的findViewById()方法可以放在该方法中。
##onPostCreate、onPostResume
onPostCreate方法当onCreate方法彻底执行完毕时回调，onPostResume当onResume方法执行完毕回调。一般不修改，在使用ActionBarDrawerToggle的使用在onPostCreate需要屏幕旋转的时候同步状态
```java
@Override
protected void onPostCreate(Bundle savedInstanceState) {
	super.onPostCreate(savedInstanceState);
	mDrawerToggle.syncState();
}
```
##onPause、onStop
onPause为失去焦点时调用，onStop则是整个窗口被完全遮盖时才会被触发
##完整生命周期为
onCreate方法会在第一次创建被执行，接着执行onState方法，也么被完全遮盖会调用onStop方法，返回的时候执行onRestart->onStart方法，结束Activity时执行onDestroy
onCreate -> onContentChanged -> onState -> onPostCreate -> onResume -> onPostResume -> onPause -> onStop -> onDestroy