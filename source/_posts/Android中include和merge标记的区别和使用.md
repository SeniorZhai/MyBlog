title: Android中include和merge标记的区别和使用
date: 2014-11-06 13:13:41
categories: Android
tags: [include,merge,复用,layout]
---
include和merge标记的作用主要是为了解决layout的重用的问题
<!--more-->
##include
比如多个Activity需要相同的标题layout即可使用include来重用
```xml
<include layout="@layout/titlebar" />
```
使用include之后，titlebar文件就会被完全嵌入到include缩指定的位置，而且也可以重新更改一些属性
```xml
<include layout="@layout/titlebar"
	android:layout_width="math_parent"
	android:layout_height="math_parent" />
```

##merge
include有一个副作用就是它需要套用一层root接点
如果不需要的话，可以使用merge标记
```xml
<merge xmlns:android="http://schemas.android.com/apk/res/android">
	<ImageView android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:src="@drawable/logo" />
</merge>
```
这样在嵌套的时候，merge标记就会被忽略掉，避免冗余的layout
