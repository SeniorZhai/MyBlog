title: ListView嵌套GridView
date: 2014-11-20 21:20:25
categories: Android
tags: [ListView,GridView,嵌套,layout,measure]
---
ListView和GridView都是可滑动控件，需要自定义GridView重写其onMeadure()
<!--more-->
```java
...
protected void onMeasure(int widthMeasureSpec,int heightMeasureSpec) {
	int expandSpec = MeasureSpec.makeMeasureSpec(Integer.MAX_VALUE >> 2,MeasureSpec.AT_MOST);
	super.onMeasure(widthMeasureSpec,expandSpec);
}
...
```
同时，GridView作为item必须设置`android:layout_height=“wrap_content”`
## 理解layout、measure
View和ViewGroup都是Android基本视图单位，ViewGroup也是一种View，但ViewGroup能够包含View
measure设置View的大小，如果有child view，循环`measure`函数
layout为摆放child view的位置
