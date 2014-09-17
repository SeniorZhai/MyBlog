title: 我眼中的REST客户端(二)
date: 2014-09-17 11:13:59
categories: Android
tags: [REST]
---
通过一个示例项目，了解如何实现REST客户端
<!--more-->
![](/img/14091701.png)

##界面实现
根据原型设计(上图)最后实现的效果如下：

![](/img/14091702.png)

![](/img/14091703.png)

- activity_mian.xml 主界面
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <FrameLayout
        android:id="@+id/content_frame"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

    <FrameLayout
        android:id="@+id/left_drawer"
        android:layout_width="175dip"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:choiceMode="singleChoice"
        />

</android.support.v4.widget.DrawerLayout>
```

- fragment_drawer.xml 左侧菜单项
```xml
<?xml version="1.0" encoding="utf-8"?>
<ListView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/listView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#e6000000"
    android:choiceMode="singleChoice"
    android:divider="@android:color/transparent"
    android:dividerHeight="0dp"
    android:orientation="vertical"
    android:paddingTop="20dip" />
```

- fragment_feed.xml 内容显示fragment
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.SwipeRefreshLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/swipe_refresh"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <com.zoe.qsbk.view.PulltoRefreshListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:choiceMode="singleChoice"
        android:divider="@android:color/transparent"
        android:dividerHeight="15dip"
        android:drawSelectorOnTop="true"
        android:fastScrollEnabled="true"
        android:footerDividersEnabled="true"
        android:headerDividersEnabled="true"
        android:paddingLeft="15dip"
        android:paddingRight="15dip"
        android:scrollbarStyle="outsideOverlay"
        android:scrollingCache="true"
        android:smoothScrollbar="true" />

</android.support.v4.widget.SwipeRefreshLayout>
```