title: Android代码-UI
date: 2015-02-08 09:08:32
categories: Android
tags: [UI,分配率]
---
<!--more-->
Android代码
1. 获取手机分辨率
```java
DisplayMetrics dm = new DisplayMetrics();
this.getWindowManager().getDefaultDisplay().getMetrics(dm);
int width = dm.widthPixels;
int height = dm.heightPixels;
```
2. dp转px
```java
public static int dip2px(Context context,float dpValue) {
	final float scale = context.getResources().getDisplayMetrics().density;
	return (dpValue *scale + 0.5f);
}
```
3. px转dp
```java
public static int px2dip(Context context,float pxValue) {
	final float scale = context.getResources().getDisplayMetrics().density;
    return (int) (pxValue / scale + 0.5f) - 15;
}
```
4. 不需要Context得到density的方法
```java
Resources.getSystem().getDisplatMetrics().density;
```