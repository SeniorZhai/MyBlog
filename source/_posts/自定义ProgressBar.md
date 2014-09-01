title: 自定义ProgressBar
date: 2014-07-14 13:39:21
categories: Android
tags: [Android]
---
- anim

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- oneshot="false" 循环播放 -->
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="false" >

    <item android:duration="60">
        <clip
            android:clipOrientation="horizontal"
            android:drawable="@drawable/loading_1"
            android:gravity="left" >
        </clip>
    </item>
    <item android:duration="60">
        <clip
            android:clipOrientation="horizontal"
            android:drawable="@drawable/loading_2"
            android:gravity="left" >
        </clip>
    </item>
    <item android:duration="60">
        <clip
            android:clipOrientation="horizontal"
            android:drawable="@drawable/loading_3"
            android:gravity="left" >
        </clip>
    </item>
    <item android:duration="60">
        <clip
            android:clipOrientation="horizontal"
            android:drawable="@drawable/loading_4"
            android:gravity="left" >
        </clip>
    </item>
    <item android:duration="60">
        <clip
            android:clipOrientation="horizontal"
            android:drawable="@drawable/loading_5"
            android:gravity="left" >
        </clip>
    </item>
    <item android:duration="60">
        <clip
            android:clipOrientation="horizontal"
            android:drawable="@drawable/loading_6"
            android:gravity="left" >
        </clip>
    </item>
    <item android:duration="60">
        <clip
            android:clipOrientation="horizontal"
            android:drawable="@drawable/loading_7"
            android:gravity="left" >
        </clip>
    </item>
    <item android:duration="60">
        <clip
            android:clipOrientation="horizontal"
            android:drawable="@drawable/loading_8"
            android:gravity="left" >
        </clip>
    </item>

</animation-list>
```

- layout

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
   >
	<!-- android:indeterminate="false" 不明确滚动的数值 -->

    <ProgressBar
        android:id="@+id/progressBar"
        android:layout_width="63dp"
        android:layout_height="63dp"
        android:layout_gravity="center"
        android:indeterminate="false"
        android:indeterminateDrawable="@anim/progress_bar_anim"
        android:scaleType="centerInside" />

</FrameLayout>
```
[例子](https://github.com/zt1991616/CustomProgressBar/)

![](https://github.com/zt1991616/blog/raw/master/Image/14080501.png)