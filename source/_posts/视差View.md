title: 视差View
date: 2015-04-16 14:38:50
categories:
tags:
---
[ParallaxEverywhere](https://github.com/Narfss/ParallaxEverywhere)是一个可以产生视差的View，可以使文本和图片在滚动的时候产生视差。
<!--more-->
- 必须在有滚动的视图内，才会有作用
- 在Image中必须设置缩放模式时必须是centerCrop、centerInside、center
- ImageView里必须有尺寸

## 导入
```
compile 'com.fmsirvent:parallaxeverywhere:1.0.4'
```
布局
```xml
   <FrameLayout
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_gravity="center"
        android:layout_margin="10dp"
        android:layout_weight="1">

        <com.fmsirvent.ParallaxEverywhere.PEWImageView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_gravity="center"
            android:layout_margin="10dp"
            android:scaleType="centerCrop"
            android:src="@drawable/alicante_explanada" />

        <com.fmsirvent.ParallaxEverywhere.PEWTextView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_margin="10dp"
            android:gravity="bottom|center_horizontal"
            android:text="@string/alicante_explanada"
            android:textColor="@android:color/white"
            app:block_parallax_x="true"
            app:parallax_x="160dp"
            app:parallax_y="160dp"
            app:reverse="reverseY" />

    </FrameLayout>
```

## 属性
- `reverse` = ["none","reverseX","reverseY","reverseBoth"] 反向滚动方向
- `block_parallax_x` and `block_parallax_y` boolean 大块滚动
- `interpolation` 滚动速率变化 ["linear", "accelerate_decelerate", "accelerate", "anticipate", "anticipate_overshoot", "bounce", "decelerate", "overshoot"] 
- `parallax_x` and `parallax_y ` 只存在图像中，设置视差方向

<https://github.com/SeniorZhai/Parallax>