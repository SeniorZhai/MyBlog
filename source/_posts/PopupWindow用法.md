title: PopupWindow用法
date: 2014-10-27 16:30:35
categories:
tags:
---
<!--more-->
```java
PopupWindow mPop = new PopupWindow(getLayoutInflater().inflate(R.layout.window,null),LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);
mPop.setAnimationStyle(R.style.AnimationPreview);
mPop.setOutsideTouchable(true);	// 点击外部是否关闭
mPop.setFocusable(false);	// 设置PopupWindow能否获得焦点
mPop.showAsDropDown(anchor,0,0);	// 设置显示PopupWindow的位置在View的下方，x,y表示坐标偏移量
mPop.showAtLocation(findViewById(R.id.parent),Gravity.LEFT,0,-90); // 以某个View为参考，表示弹出窗口以parent组件为参考，位于左侧，偏移-90
mPop.setOnDismissListenerd(new PopupWindow.OnDismissListener(){});	// 设置窗口消失事件
```
