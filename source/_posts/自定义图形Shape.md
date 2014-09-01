title: 自定义图形Shape
date: 2014-06-17 13:26:23
categories: Android
tags: [Android]
---
```xml
<shape>
	<!-- 填充 -->
	<solid android:color="#ff9d77">
	<!-- 渐变 -->
	<gradient          
		android:startColor="#ff8c00"     
      	android:endColor="#FFFFFF"     
     	android:angle="270" />     
 	<!-- 描边 -->      
 	<stroke     
       android:width="2dp"     
       android:color="#dcdcdc" />        
  	<!-- 圆角 -->     
  	<corners android:radius="2dp" />     
  	<padding     
       android:left="10dp"           
       android:top="10dp"     
       android:right="10dp"     
       android:bottom="10dp" />  
```
solid：填充
gradient：渐变
	startColor和endColor分别为起始和结束颜色
	angle是渐变角度，必须是45的整倍数
	type是渐变模式，可选linear，即线性渐变；也可以为radial径向渐变，径向渐变需要制定半径gradientRadius="50"
strokr:描边
	width指宽度，color值颜色，dashWitdh指定虚线宽，dashGap指定相隔距离
corners:原件
	radius角的弧度，值越大角越圆