title: ToolBar的使用
date: 2015-01-07 09:54:45
categories: Android
tags: [ToolBar]
---
ToolBar属于ActionBar的升级版，扩展了ActionBar，使得我们可以像使用独立的控件一样使用ToolBar。
<!--more-->
##风格(Style)
```xml
<style name="AppTheme.Base" parent="Theme.AppCompat">
	<item name="windowActionBar">false</item>
	<item name="android:windowNoTitle">true</item>
</style>
```
##界面(Layout)
```xml
<android.support.v7.widget.Toolbar
	android:id="@+id/toolbar"
	android:layout_height="wrap_content"
	android:layout_width="match_parent"
	android:minHeight="?attr/actionBarSize" />
```

- colorPrimaryColor Toolbar的颜色
	+ 在layout文件中设置background属性
- colorPrimaryDark 状态栏背景色
- textColorPrimary 标题和更多菜单中文字的颜色
- colorAccent 可勾选控件被选择的颜色
- colorControlNormal 各控件预设的颜色
- windowBackground App的背景色
- navigationBarColor 导航栏的背景色(API Level需要大于)
	
	注：以上在style中设置

##配合DrawerLayout使用
```java
setSupportActionBar(mToolbar);
mDrawerLayout = (DrawerLayout) findViewId(R.id.drawer);
mDrawerToggle = new ActionBarDrawerToggle(this, mDrawerLayout, mToolbar, R.string.drawer_open,
                R.string.drawer_close);
mDrawerLayout.setDrawerListener(mDrawerToggle);
```

###示例
- 布局
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">


    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:background="#2196F3"
        style="@style/ToolBarStyle"
        android:minHeight="?attr/actionBarSize" />

    <android.support.v4.widget.DrawerLayout
        android:id="@+id/drawerlayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent">

        </LinearLayout>
        <LinearLayout
            android:layout_width="200dp"
            android:background="@android:color/white"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            ></LinearLayout>
    </android.support.v4.widget.DrawerLayout>
</LinearLayout>
```
- style
```xml
<resources xmlns:android="http://schemas.android.com/apk/res/android">

    <style name="AppBaseTheme" parent="Theme.AppCompat.Light.NoActionBar">

        <!-- toolbar（actionbar）颜色 -->
        <item name="colorPrimary">#4876FF</item>
        <!-- 状态栏颜色 -->
        <item name="colorPrimaryDark">#3A5FCD</item>
        <!-- 窗口的背景颜色 -->
        <item name="android:windowBackground">@color/backgroud</item>
        <!-- 标题颜色 -->
        <item name="android:textColorPrimary">#FFFFFF</item>
        <!-- 箭头 -->
        <item name="drawerArrowStyle">@style/AppTheme.DrawerArrowToggle</item>
        <!-- 菜单按钮 -->
        <item name="actionOverflowButtonStyle">@style/AppTheme.overflow</item>
    </style>

    <style name="AppTheme" parent="@style/AppBaseTheme"/>

    <!-- 箭头颜色 -->
    <style name="AppTheme.DrawerArrowToggle" parent="Base.Widget.AppCompat.DrawerArrowToggle">
        <item name="color">@android:color/white</item>
    </style>

    <!-- 替换菜单文件 -->
    <style name="AppTheme.overflow" parent="Widget.AppCompat.ActionButton.Overflow">
        <item name="android:src">@drawable/android</item>
    </style>

    <!-- popup style -->
    <style name="ToolBarStyle" parent="">
        <item name="popupTheme">@style/ThemeOverlay.AppCompat.Light</item>
    </style>

</resources>
```
Java代码
```java
public class MainActivity extends ActionBarActivity implements DrawerLayout.DrawerListener {

    private DrawerLayout drawerLayout;
    private Toolbar toolbar;
    private ActionBarDrawerToggle toggle;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        toolbar = $(R.id.toolbar);
        drawerLayout = (DrawerLayout) findViewById(R.id.drawerlayout);
        toolbar.setTitle("Title");
        setSupportActionBar(toolbar);
        toggle = new ActionBarDrawerToggle(this, drawerLayout, toolbar, R.string.app_name, R.string.action_settings);
        toggle.syncState();
        drawerLayout.setDrawerListener(toggle);
    }


    protected <T extends View> T $(int id) {
        return (T) findViewById(id);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();
        if (id == R.id.action_settings) {
            Toast.makeText(this, "setting", Toast.LENGTH_SHORT).show();
            return true;
        }
        return super.onOptionsItemSelected(item);
    }

    @Override
    protected void onPostCreate(Bundle savedInstanceState) {
        super.onPostCreate(savedInstanceState);
        toggle.syncState();
    }

    @Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
        toggle.onConfigurationChanged(newConfig);
    }

    // 滑动时回调
    @Override
    public void onDrawerSlide(View view, float v) {

    }

    // 打开时回调
    @Override
    public void onDrawerOpened(View view) {

    }

    // 关闭时回调
    @Override
    public void onDrawerClosed(View view) {

    }

    // 改变状态时回调
    @Override
    public void onDrawerStateChanged(int i) {

    }
}
```