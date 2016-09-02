title: Material
date: 2014-10-14 13:14:42
categories: Android
tags: [Material]
---
<!--more-->
## Material Theme

![](/img/14101501.png)
Material Theme的定义如下
- @android:style/Theme.Material
- @android:style/Theme.Material.Light
- @android:style/Theme.Material.DarkActionBar

### 自定义主题的基础颜色
```xml
<resources>
	<style name="AppTheme" parent="android:Theme.Material">
		<item name="android:colorPrimary">@color/primary</item>
		<item name="android:colorPrimaryDark">@color/primary_dark</item>
		<item name="android:colorAccent">@color/accent</item>
	</style>
</resources>
```
