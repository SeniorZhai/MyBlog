title: 十大Material Design项目
date: 2014-11-24 11:05:41
categories: Android
tags: [Material Design,Android 5.0]
---
<!--more-->
1. [MaterialDesignLibrary](https://github.com/navasmdc/MaterialDesignLibrary)
MaterialDesignLibrary可以说是颇受开发者瞩目的一个控件效果库，能够让开发者在Android 2.2系统上使用Android 5.0才支持的控件效果，比如扁平、矩形、浮动按钮，复选框以及各式各样的进度指示器等。
MaterialDesignLibrary还拥有SnackBar、Dialog、Color selector组件，可非常便捷地对应用界面进行设置。
![](https://github.com/navasmdc/MaterialDesignLibrary/raw/master/images/logo.png)
进度指示器样式效果设置：
```xml
<com.gc.materialdesign.views.ProgressBarCircularIndetermininate    
                android:id="@+id/progressBarCircularIndetermininate"    
                android:layout_width="32dp"    
                android:layout_height="32dp"    
                android:background="# 1E88E5" />  
```
Dialog
```java
Dialog dialog = new Dialog(Context context,String title, String message);  
dialog.show();  
```
2. [RippleEffect](https://github.com/traex/RippleEffect)
来自法兰西的Robin Chutaux开发的RippleEffect基于MIT许可协议开源，能够在Android API 9+上实现Material Design，为开发者提供了一种极为简易的方式来创建带有可扩展视图的header视图，并且允许最大程度上的自定制。
![](https://github.com/traex/RippleEffect/raw/master/demo.gif)
用法（在XML文件中声明一个RippleView）：
```xml
<com.andexert.library.RippleView  
  android:id="@+id/more"  
  android:layout_width="?android:actionBarSize"  
  android:layout_height="?android:actionBarSize"  
  android:layout_toLeftOf="@+id/more2"  
  android:layout_margin="5dp"  
  ripple:rv_centered="true">  
  
  <ImageView  
    android:layout_width="?android:actionBarSize"  
    android:layout_height="?android:actionBarSize"  
    android:src="@android:drawable/ic_menu_edit"  
    android:layout_centerInParent="true"  
    android:padding="10dp"  
    android:background="@android:color/holo_blue_dark"/>  
  
</com.andexert.library.RippleView>  
```
3. [MaterialEditText](https://github.com/rengwuxian/MaterialEditText)
AppCompat v21也为开发者提供了Material Design的控件外观支持，其中就包括EditText，但却并不好用，没有设置颜色的API，也没有任何Google Material Design Spec中提到的特性。于是，来自国内的开发者“扔物线”开发了MaterialEditText库，直接继承EditText，无需修改Java文件即能实现自定义控件颜色。
![](https://github.com/rengwuxian/MaterialEditText/raw/master/images/material_edittext.png)
自定义Base Color：
```xml
app:baseColor="# 0056d3"  
```
自定义Error Color：
```xml
app:maxCharacters="10"  
app:errorColor="# ddaa00"  
```
4. [Android-LollipopShowcase](https://github.com/mikepenz/Android-LollipopShowcase)
Android-LollipopShowcase是由来自奥地利的移动、后端及Web开发者Mike Penz所开发的演示应用，集中演示了新Material Design中所有的UI效果，以及Android Lollipop中其他非常酷炫的特性元素，比如Toolbar、RecyclerView、ActionBarDrawerToggle、Floating Action Button（FAB）、Android Compat Theme等。
![](https://raw.githubusercontent.com/mikepenz/Android-LollipopShowcase/master/DEV/screenshots/screenshot_1_small.jpg)
5. [MaterialList](https://github.com/dexafree/MaterialList)
MaterialList是一个能够帮助所有Android开发者获取谷歌UI设计规范中新增的CardView（卡片视图）的开源库，支持Android 2.3+系统。作为ListView的扩展，MaterialList可以接收、存储卡片列表，并根据它们的Android风格和设计模式进行展示。此外，开发者还可以创建专属于自己的卡片布局，并轻松将其添加到CardList中。
```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:paddingLeft="@dimen/activity_horizontal_margin"  
    android:paddingRight="@dimen/activity_horizontal_margin"  
    android:paddingTop="@dimen/activity_vertical_margin"  
    android:paddingBottom="@dimen/activity_vertical_margin">  
  
    <com.dexafree.materiallistviewexample.view.MaterialListView  
        android:layout_width="fill_parent"  
        android:layout_height="fill_parent"  
        android:id="@+id/material_listview"/>  
  
</RelativeLayout>  
```
6. [android-floating-action-button](https://github.com/futuresimple/android-floating-action-button)
Floating Action Button（FAB）是众多专家大牛针对Material Design讨论比较细化的一个点，通过圆形元素与分割线、卡片、各种Bar的直线形成鲜明对比，并使用色彩设定中鲜艳的辅色，带来更具突破性的视觉效果。也正因如此，在Github上，有着许多与FAB相关的开源项目，基于Material Design规范的开源Android浮动Action Button控件android-floating-action-button便是其中之一。
![](https://github.com/futuresimple/android-floating-action-button/raw/master/screenshots/menu.gif)
其主要特性如下：
- 支持常规56dp和最小40dp的按钮；
- 支持自定义正常、Press状态以及可拖拽图标的按钮背景颜色；
- AddFloatingActionButton类能够让开发者非常方便地直接在代码中写入加号图标；
- FloatingActionsMenu类支持展开/折叠显示动作。
7. [android-ui](https://github.com/markushi/android-ui)
android-ui是Android UI组件类库，支持Android API 14+，包含了ActionView、RevealColorView等UI组件。其中，ActionView可使Action动作显示动画效果，而RevealColorView则带来了Android 5.0中的圆形显示/隐藏动画体验。
![](https://raw.githubusercontent.com/markushi/android-ui/master/example-action.gif)
8. [Material Menu](https://github.com/balysv/material-menu)
Material Menu为开发者带来了非常酷炫的Android菜单、返回、删除以及检查按钮变形，完全控制动画，并为开发者提供了两种MaterialMenuDrawable包装。
![](https://camo.githubusercontent.com/642bd91749dce58abfba00fe1cefdf2cf4213fd3/68747470733a2f2f7261772e6769746875622e636f6d2f62616c7973762f6d6174657269616c2d6d656e752f6d61737465722f6172742f64656d6f2e676966)
自定义颜色等操作：
```java
// change color  
MaterialMenu.setColor(int color)  
  
// change transformation animation duration  
MaterialMenu.setTransformationDuration(int duration)  
  
// change pressed animation duration  
MaterialMenu.setPressedDuration(int duration)  
  
// change transformation interpolator  
MaterialMenu.setInterpolator(Interpolator interpolator)  
  
// set RTL layout support  
MaterialMenu.setRTLEnabled(boolean enabled)  
```
9. [Android-ObservableScrollView](https://github.com/ksoichiro/Android-ObservableScrollView)
Android-ObservableScrollView是一款用于在滚动视图中观测滚动事件的Android库。它能够轻而易举地与Android 5.0 Lollipop引进的工具栏（Toolbar）进行交互，还可以帮助开发者实现拥有Material Design应用视觉体验的界面外观，支持ListView、ScrollView、WebView、RecyclerView、GridView组件。
![](https://github.com/ksoichiro/Android-ObservableScrollView/raw/master/observablescrollview-samples/demo12.gif)
交互代码回调：
```java
@Override  
public void onUpOrCancelMotionEvent(ScrollState scrollState) {  
    ActionBar ab = getSupportActionBar();  
	if (scrollState == ScrollState.UP) {  
        if (ab.isShowing()) {  
            ab.hide();  
        }  
    } else if (scrollState == ScrollState.DOWN) {    
        if (!ab.isShowing()) {  
                ab.show();  
        }  
    }  
}  
```
10. [Material Design Icons](https://github.com/google/material-design-icons)
最后，再来介绍一下Google Material Design规范的官方开源图标集Material Design Icons。良心Google开源了包括Material Design系统图标包在内的750个字形，涵盖动作、音视频、通信、内容、编辑器、文件、硬件、图像、地图、导航、通知、社交等各个方面，适用于Web、Android和iOS应用开发，绝对是开发者及设计师必备的资源。
图标格式主要包括： 
- SVG格式，24px和48px；
- SVG和CSS Sprites；
- 适用于Web平台的1x、2x PNG格式图标；
- 适用于iOS的1x、2x、3x PNG图标；
- 所有图标的Hi-dpi版本（hdpi、mdpi、xhdpi、xxhdpi、xxxhdpi）。