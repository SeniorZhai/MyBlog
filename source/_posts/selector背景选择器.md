title: selector背景选择器
date: 2014-08-11 13:54:57
categories: Android
tags: [Android]
---
- listview的item可选的背景

```xml
<?xml version="1.0" encoding="utf-8" ?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
	<!-- 默认时的背景背景 -->
	<item android:drawable="@drawable/pic1" />
	<!-- 没有焦点时的背景图片 -->
	<item android:state_window_focused="false" android:drawable="@drawable/pic1" />
	<!-- 非触摸模式下获得焦点并单击时的背景图片 -->
	<item android:state_focused="true" android:state_pressed="true" android:drawable="@drawable/pic2" />
	<!-- 触摸模式下单击时的背景图片 -->
	<item android:state_focused="false" android:state_pressed="true" android:drawable="@drawable/pic3" />
	<!-- 选中时的图片背景 -->
	<item android:state_selected="true" android:drawable="@drawable/pic4" />
	<!-- 获得焦点时的图片背景 -->
	<item android:state_focused="true" android:drawable="@drawable/pic5" />
```
在listView中配置`android:listSelector="@drawable/list_item_bg"`或者在item中添加`android:background="@drawable/list_item_bg"`即可实现，或者在java代码中使用`Drawable drawable = getResources().getDrawable(R.drawable.list_item_bg);`,`listView.setSelector(drawable);`。可能出现列表出现黑的情况，加上`android:cacheColorHint="@android:color/transparent"`。

- button的可选背景
	+ android:state_selected 为选中
	+ android:state_focused 获得焦点
	+ android:state_pressed 点击
	+ android:state_enable 是否响应事件，指所有事件
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
	<item android:state_selected="true" android:color="#FFF" />
	<item android:state_focused="true" android:color="FFF" />
	<item android:state_pressed="true" android:color="#FFF" />
	<item android:color="#000" />
</selector>
``` 