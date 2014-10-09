title: ColorPicker
date: 2014-10-09 09:50:13
categories: Android
tags: [颜色选择器,动画]
---
一个简单的带动画的颜色选择Dialog
<!--more-->
![](/img/14100901.png)
- 使用
```java
new AnimatedColorPickDialog.Builder(MainActivity.this)
	.setTitle("选择一个颜色")
	.setColors(styleColors)	//颜色数组 RGB十六进制整数格式
	.setOnColorClickListener(new AnimatedColorPickerDialog.ColorClickListener() {
		@Override
		public void onColorClick(int color) {
			colorDisplayView.setBackgroundColor(color);
		}
	}).create().show();
```
示例:<https://github.com/SeniorZhai/ColorPickDialog>