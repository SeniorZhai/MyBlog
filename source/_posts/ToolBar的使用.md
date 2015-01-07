title: ToolBar的使用
date: 2015-01-07 09:54:45
categories:
tags:
---
<!--more-->
##风格(Style)
```xml
<style name="AppTheme.Base" parent="Theme.AppCompat">
	<item name="windowActionBar">false</item>
	<item name="android:windowNoTitle">true</item>
</style>
```

##界面(Layout)
```xml
<android.support.v7.widget.Toolbar
	android:id="@+id/toolbar"
	android:layout_height="wrap_content"
	android:layout_width="match_parent"
	android:minHeight="?attr/actionBarSize" />
```


- colorPrimaryDark 状态栏背景色
- textColorPrimary 标题和更多菜单中文字的颜色
- colorPrimaryColor Toolbar的颜色
- colorAccent 可勾选控件被选择的颜色
- colorControlNormal 各控件预设的颜色
- windowBackground App的背景色
- navigationBarColor 导航栏的背景色(API Level需要大于21)