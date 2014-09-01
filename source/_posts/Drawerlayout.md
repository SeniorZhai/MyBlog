title: Drawerlayout
date: 2014-07-04 13:35:17
categories: Android
tags: [Android]
---
![](https://github.com/zt1991616/blog/raw/master/Image/14070401.png)

##使用
`DrawerLayout`类在`Support Library`里，有`android-support-v4.jar`这个包即可

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.widget.DrawerLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/drawer_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent" >

        <FrameLayout
            android:id="@+id/content_frame"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />


        <ListView
            android:id="@+id/left_drawer"
            android:layout_width="240dp"
            android:layout_height="match_parent"
            android:layout_gravity="left"
            android:background="#111"
            android:choiceMode="singleChoice"
            android:divider="@android:color/transparent"
            android:dividerHeight="0dp" />
    </android.support.v4.widget.DrawerLayout>

</RelativeLayout>
```