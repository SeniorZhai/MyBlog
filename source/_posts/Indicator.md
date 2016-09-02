title: Indicator
date: 2014-08-28 11:00:21
categories: Android
tags: [Android,Indicator]
---
提供了一个水平滚动的Indicator
<!--more-->
[viewflow](https://github.com/pakerfeldt/android-viewflow)

## 绑定
在java中绑定监听
```java
viewFlow.setAdapter(new ImageAdapter(this),5);
CircleFlowIndicator indic = (CircleFlowIndicator) findViewById(R.id.viewflowindic);
viewFlow.setFlowIndicator(indic);
```
## 自定义属性
+ activeColor 选中的颜色
+ inactiveColor 未选中的颜色
+ activeType 选中模式(fill填充 or stroke描边)
+ inactiveType 选中模式(fill填充 or stroke描边)
+ fadeOut 淡出时间(0为不淡出)
+ radius 半径
```xml
<...CircleFlowIndicator
	...
	app:fadeOut=1000
	app:radius="5dp"
	app:activeType="fill"
	app:activeColor="@color/blue"
	app:inactiveType="stroke"
	app:inactiveColor="@color/white"
	/>
```
[](https://github.com/zt1991616/ViewFlowDemo)

![](/img/14082801.png)