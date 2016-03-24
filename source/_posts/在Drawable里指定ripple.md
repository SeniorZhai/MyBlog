title: 在Drawable里指定ripple
date: 2016-03-15 11:05:29
categories: Android
tags: [ripple]
---
<!--more-->
在`drawable`中
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true">
        <shape android:shape="rectangle">
            <solid android:color="@android:color/darker_gray" />
        </shape>
    </item>
    <item>
        <shape android:shape="rectangle">
            <solid android:color="@android:color/white" />
        </shape>
    </item>
</selector>
```
在`drawable-v21`中
```xml
<?xml version="1.0" encoding="utf-8"?>
<ripple xmlns:android="http://schemas.android.com/apk/res/android"
    android:color="@android:color/darker_gray">
    <item>
        <shape android:shape="rectangle">
            <solid android:color="@android:color/white" />
        </shape>
    </item>
</ripple>
```
