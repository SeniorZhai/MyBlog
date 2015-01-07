title: ToolBar的使用
date: 2015-01-07 09:54:45
categories: Android
tags: [ToolBar]
---
ToolBar属于ActionBar的升级版，扩展了ActionBar，使得我们可以像使用独立的控件一样使用ToolBar。
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

- colorPrimaryColor Toolbar的颜色
	+ 在layout文件中设置background属性
- colorPrimaryDark 状态栏背景色
- textColorPrimary 标题和更多菜单中文字的颜色
- colorAccent 可勾选控件被选择的颜色
- colorControlNormal 各控件预设的颜色
- windowBackground App的背景色
- navigationBarColor 导航栏的背景色(API Level需要大于)
	
	注：以上在style中设置

##配合DrawerLayout使用
```java
setSupportActionBar(mToolbar);
mDrawerLayout = (DrawerLayout) findViewId(R.id.drawer);
mDrawerToggle = new ActionBarDrawerToggle(this, mDrawerLayout, mToolbar, R.string.drawer_open,
                R.string.drawer_close);
mDrawerLayout.setDrawerListener(mDrawerToggle);
```