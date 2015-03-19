title: Android Shape的使用
date: 2015-03-17 15:20:02
categories: Android
tags: [shape]
---
<!--more-->
- shape 
	android:shape["rectagle"|"oval"|"line"|"ring"]
	+ rectagle 矩形
    + oval椭圆
    + line水平直线
    + ring环形
- gradient 渐变
	+ android:startColor 起始颜色
	+ android:endColor 结束颜色
	+ android:angle 渐变角度，0从上到下，90从左到右，数值必须为45的倍数，默认为0
	+ android:type 渐变样式 line线性渐变，radial环形渐变，sweep扫描渐变
- solid 填充颜色
	+ android:color 填充颜色
- stroke 描边
	+ android:width 描边的宽度
	+ android:color 描边的颜色
	+ android:dashWidth 间隔线的宽度
	+ android:dashGap 间隔线的间隔
- corners  圆角
	+ android:radius 圆角的半径，值越大越圆
	+ android:topRightRadius 右上角半径
	+ android:bottomLeftRadius 左下角半径
	+ android:topLeftRadius 右上角半径
	+ android:bottomRightRadius 右下角半径
