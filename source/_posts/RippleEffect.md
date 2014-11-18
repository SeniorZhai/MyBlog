title: RippleEffect
date: 2014-11-08 14:52:50
categories: Android
tags: [MaterialDesign,Animation,波纹,按钮]
---
<!--more-->
![](/img/14110801.gif)
```xml
<com.andexert.library.RippleView
        android:id="@+id/more"
        ripple:rv_centered="true"
        android:layout_width="120dp"
        android:layout_height="60dp"
        android:layout_margin="5dp">

        <ImageView
            android:layout_width="120dp"
            android:layout_height="60dp"
            android:layout_centerInParent="true"
            android:background="@android:color/holo_blue_light"
            android:padding="10dp"
            android:src="@android:drawable/ic_menu_edit" />

</com.andexert.library.RippleView>
```
##定制属性
- app:rv_alpha:波纹的透明度，integer类型，默认为90，范围0-255
- app:rv_framerate:波纹放大过程中的步进，integer类型，默认10
- app:rv_rippleDuration:波纹动画的间隔，integer类型，默认400
- app:paddingStart:波纹其实padding
- app:paddingEnd:波纹结束padding
- app:rv_color:波纹的颜色，color类型，默认是白色
- app:rv_centered:是否在控件的最中间，boolean类型，默认false
- app:rv_type:波纹类型，enum类型(simpleRipple,doubleRipple,rectangle)，默认simpleRipple
- app:rv_zoom:是否开启放大动画，boolean类型，默认false
- app:rv_zoomDuration:放大动画的时间，integer类型，默认150
- app:rv_zoomScale:放大动画，integer类型，默认1.03

