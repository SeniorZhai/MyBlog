title: ArcMenu
date: 2014-08-15 14:04:00
categories: Android
tags: [Android]
---
仿Path带动画效果的扇形菜单

![](https://github.com/zt1991616/blog/raw/master/Image/14081503.png)
---
## 用法
```java
ArcMenu menu = (ArcMenu) findViewById(R.id.arc_menu);
final int itemCount = ITEM_DRAWABLES.length;
for (int i = 0; i < itemCount; i++) {
	ImageView item = new ImageView(this);
	item.setImageResource(ITEM_DRAWABLES[i]);
	menu.addItem(item,new OnClick(View v){
		Toast.makeText(MainActivity.this, "position:" + position, Toast.LENGTH_SHORT).show();
	});
}
```
改变外观可以在xml中
```xml
custom:childSize="50px"
custom:fromDegrees="0.0"
custom:toDegrees="300.0"
```
或者在java中
```java
arcLayout.setChildSize(50);
arcLayout.setArc(0.0f,300.0f);
```
---
[ArcMenu](https://github.com/zt1991616/Arc)