title: Action Bar
date: 2014-04-14 13:18:52
categories: Android
tags: [Android]
---
## 添加Action Bar
### Android 3.0及以上版本
Android 3.0（API level 11）开始，所有使用Theme.Holo主题的Activity都内置了action bar，此主题是targetSdkVersion或minSdkVersion属性大于等于11时的默认主题。
```xml
<manifest ...>
    <uses-sdk android:minSdkVersion="11" .../>
</manifest>
```
>如果创建一个自定义主题，需呀确保它使用一个`Theme.Holo`主题作为父主题

### 支持Android 2.1及以上版本
可以通过天剑`Support库`或者`v7 appcompat`库集成Action Bar到低版本
1. activity继承ActionBarActivity
```java
public class MainActivity extends ActionBarActivity {...}
```
2. 在mainfest文件中设置application或设置单独的activity的主题为`Theme.AppCompat`
```xml
<activity android:theme="@style/Theme.AppCompat.Light" ... >
```

## 移除action bar
可以把该Activity的主题设置为`Theme.Holo.NoActionBar`
```xml
<activity android:theme=@"android:style/Theme.Holo.NoActionBar">
```
也可以在运行时通过调用`hide()`来隐藏action bar
```java
ActionBar actionBar = getActionBar();
actionBar.hide();
```
也可以用`show()`来重新显示ActionBar。

## 添加Action项

当acitivity第一次被启动时，系统会调用activity中的onCreateOptionsMenu()来构建Action和滑动菜单。菜单项通过XML格式的菜单资源来包装。
在此XML文件中，通过将<item>元素声明为带`android:showAsAction="ifRoom"`属性，你可以让一个菜单项显示为action项。通过这种方式，菜单项将会仅当有空间时才显示在action bar中，便于快速访问。如果没有空间的话，菜单项将会显示在滑动菜单中。
如果菜单项同时具有标题和文本——带有`android:title`和`android:icon`属性——则action项默认只显示图标。如果需要显示文本标题，只要在`android:showAsAction`属性中加入`withText`即可
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
	<item android:id=@"+id/menu_save"
		android:icon=@"drawable/ic_menu_save"
		android:title=@"string/menu_save"
		android:showAsAction="ifRoom|withText"/>
</menu>
```
在Activity中实现`onCreateOptionMenu()`回调方法来`inflate`菜单资源从而获取Menu对象。
```java
@Override
public boolean onCreateOptionsMenu(Menu menu)
{
	MenuInflater inflater = getMenuInflater();
	inflater.inflate(R.menu.main_action_menu,menu);
	return super.onCreateOptionsMenu(menu);
}
```
用户选中一个action项，activity将会收到一个对onPtionsItemSelected()调用，并传入android:id属性给出的ID——选项菜单中的所有项都会被收到该回调方法。
```java
@Override
public boolean onOptionsItemSelected(MenuItem item){
    switch (item.getItemId()){
        case R.id.action_search:
            openSearch();
            return true;
        case R.id.action_settings:
            openSettings();
            return true;
        default:
            return super.onOptionsItemSelected(item);
    }
}
```

## 使用split action bar
Android 4.0(API level 14)及以上版本时，action bar增加一个名为"split action bar"的模式。启用split action bar后，如果在窄屏上（比如纵向显示的手持设备）运行activity，屏幕底部将会显示一个单独的bar，用于显示所有的action项，把action bar拆分开、让多个action项分开显示，可以确保在窄屏上以合理的间隔显示所有的action项，并为顶部的导航条和标题栏留下足够的空间。
启用split action bar只要把uiOptions="splitActionBarWhenNarrow"加入你的<activity>或<application>manifest元素即可。

## 用应用程序图标导航
默认情况下，应用程序图标显示在action bar的左侧。如果需要，可以把让图标成为一个action项。作为用户对图标Action的响应，应用程序必须执行以下两件事之一：
- 返回应用程序的初始`home activity`
- 应用程序向上回退至上一层
当用户触摸图标时，系统会调用Activity的`onOptionsItemSelected()`方法，并带入`android.R.id.home`ID，作为响应，你应该启动home activity或让用户回到应用程序的上一侧。
如果你把返回home activity作为应用程序图标的响应，则你应该在`Intent`中包含`FLAG_ACTIVITY_CLEAR_TOP`标志。如果你要启动的Activity已经存在于当前任务中了，则用这个标志，可以将所有在其之上的Activity都销毁，并把该activity推到前台。因为回到`home`等同于`返回`的Action，通常不应该创建一个新的home activity实例，所以加入这个标志是非常重要的。否则的话，当前的任务栈中可能会有一长串activity，其中包含了home activity的多个实例。
```java
@Override
public boolean onOptionsItemSeleted(MenuItem item) {
	switch(item.getItemId()) {
		case android.R.id.home:
			Intent intent = new Intent(this,HomeActivity.class);
			intent.addFlags(Intent.FLAG_ACTIVITY_CLER_TOP);
			startActivity(intent);
			return true;
	}
	default:
		return super.onPtionsItemSelected(item);
}
```
用户可以从其他应用程序中进入当前Activity，这时你也许还想加入`FLAG_ACTIVITY_NEW_TASK`标志，次标志表示，不管用户导航至`home`还是返回上一层，新的activity都不加入当前任务栈，而是放入一个属于你的应用程序的栈。比如说，如果用户通过其他应用提交一个itent，启动了一个属于你的应用的activity，然后选中了action bar图标来回到home或向上回退一级，`FLAG_ACTIVITY_CLEAR_TOP`标志将会启动属于你的应用程序任务中的Activity(而不是当前任务)。系统或是启动一个新的任务并把这个你的这个新activity作为根activity，或者，假如后台任务中存在该activity的实例的话，则把任务推到前台，该activity会收到`onNewIntent()`。因此，如果你的activity要从其他应用接收intent(声明了任何通用的intent过来器)，你通常应该在intent中加入`FLAG_ACTIVITY_NEW_TASK`：
```
	intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP | Intent.FLAG_ACTIVITY_NEW_TASK);
```
>如果用图标来返回home activity，请注意自Android 4.0(API level 14)开始，你必须显式地调用`setHomeButtonEnabled(true)`将图标用作action项（之前的版本中，图标默认就是作为action项使用的）。

## 向上导航
调用`ActionBar`的setDisplayHomeAsEnabled(true)
```java
protected void onCreate(Bundle savedInstanceState){
	super.onCreate(savedInstanceState);

	setContentView(R.id.main);
	ActionBar actionBar = getActionBar();
	actionBar.setDisplayHomeAsUpEnabled(true);
	...
}
```
当用户触摸图标时，系统会调用activity的`onOptionsItemSelected()`方法，并带入`android.R.id.home`ID.
记得在`Intent`上使用`FLAG_ACTIVITY_CLEAR_TOP`标志，这样就不会创建已存在的父activity的新实例。
在`manifest`文件中声明父Activity
```xml
<application ...>
    <activity
        android:name="com.example.myfirstapp.MainActivity" ...>
        ...
    </activity>
    <activity
        android:name="com.example.myfirstapp.DisplayMessageActivity"
        android:label="@string/title_activity_display_message"
        android:parentActivityName="com.example.myfirstapp.MainActivity" >
        <meta-data
            android:name="android.support.PARENT_ACTIVITY"
            android:value="com.example.myfirstapp.MainActivity" />
    </activity>
</application>
```
## ActionBar风格化
Android的基本主题有暗、淡两个

![](https://github.com/zt1991616/blog/raw/master/Image/14072801.png)
![](https://github.com/zt1991616/blog/raw/master/Image/14072802.png)

在manifest文件中设置`application`或`activity`的`android:theme`属性
```xml
<application android:theme="@android:style/Theme.Holo.Light" ... />
```
![](https://github.com/zt1991616/blog/raw/master/Image/14072803.png)

使用Support库时，必须使用`Theme.AppCompat`主题替代
- Theme.AppCompat，一个“暗”的主题
- Theme.AppCompat.Light，一个“淡”的主题
- Theme.AppCompat.Light.DarkActionBar，一个带有“暗” action bar 的“淡”主题
### 自定义
为了自定义主题，通过重写actionBarStyle属性来改变action bar的背景。通过指定一个drawable资源来重写background属性。

![](https://github.com/zt1991616/blog/raw/master/Image/14072804.png)

Android 3.0可以这样写
res/values/themes.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- the theme applied to the application or activity -->
    <style name="CustomActionBarTheme"
           parent="@android:style/Theme.Holo.Light.DarkActionBar">
        <item name="android:actionBarStyle">@style/MyActionBar</item>
    </style>

    <!-- ActionBar styles -->
    <style name="MyActionBar"
           parent="@android:style/Widget.Holo.Light.ActionBar.Solid.Inverse">
        <item name="android:background">@drawable/actionbar_background</item>
    </style>
</resources>
```
然后，将你的主题应该到你的 app 全局或单个的 activity 之中：
```xml
<application android:theme="@style/CustomActionBarTheme" ... />
```
Android 2.1 版本可以这么写
res/values/themes.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- the theme applied to the application or activity -->
    <style name="CustomActionBarTheme"
           parent="@style/Theme.AppCompat.Light.DarkActionBar">
        <item name="android:actionBarStyle">@style/MyActionBar</item>

        <!-- Support library compatibility -->
        <item name="actionBarStyle">@style/MyActionBar</item>
    </style>

    <!-- ActionBar styles -->
    <style name="MyActionBar"
           parent="@style/Widget.AppCompat.Light.ActionBar.Solid.Inverse">
        <item name="android:background">@drawable/actionbar_background</item>

        <!-- Support library compatibility -->
        <item name="background">@drawable/actionbar_background</item>
    </style>
</resources>
```
然后，将你的主题应该到你的 app 全局或单个的 activity 之中：
```xml
<application android:theme="@style/CustomActionBarTheme" ... />
```
### 自定义本文颜色
- action bar的标题：指定`textColor属性`
- action bar的页签：重写 actionBarTabTextStyle
- action bar按钮：重写 actionMenuTextColor
Android 3.0
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- the theme applied to the application or activity -->
    <style name="CustomActionBarTheme"
           parent="@style/Theme.Holo">
        <item name="android:actionBarStyle">@style/MyActionBar</item>
        <item name="android:actionBarTabTextStyle">@style/MyActionBarTabText</item>
        <item name="android:actionMenuTextColor">@color/actionbar_text</item>
    </style>

    <!-- ActionBar styles -->
    <style name="MyActionBar"
           parent="@style/Widget.Holo.ActionBar">
        <item name="android:titleTextStyle">@style/MyActionBarTitleText</item>
    </style>

    <!-- ActionBar title text -->
    <style name="MyActionBarTitleText"
           parent="@style/TextAppearance.Holo.Widget.ActionBar.Title">
        <item name="android:textColor">@color/actionbar_text</item>
    </style>

    <!-- ActionBar tabs text styles -->
    <style name="MyActionBarTabText"
           parent="@style/Widget.Holo.ActionBar.TabText">
        <item name="android:textColor">@color/actionbar_text</item>
    </style>
</resources>
```
Android 2.1 
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- the theme applied to the application or activity -->
    <style name="CustomActionBarTheme"
           parent="@style/Theme.AppCompat">
        <item name="android:actionBarStyle">@style/MyActionBar</item>
        <item name="android:actionBarTabTextStyle">@style/MyActionBarTabText</item>
        <item name="android:actionMenuTextColor">@color/actionbar_text</item>

        <!-- Support library compatibility -->
        <item name="actionBarStyle">@style/MyActionBar</item>
        <item name="actionBarTabTextStyle">@style/MyActionBarTabText</item>
        <item name="actionMenuTextColor">@color/actionbar_text</item>
    </style>

    <!-- ActionBar styles -->
    <style name="MyActionBar"
           parent="@style/Widget.AppCompat.ActionBar">
        <item name="android:titleTextStyle">@style/MyActionBarTitleText</item>

        <!-- Support library compatibility -->
        <item name="titleTextStyle">@style/MyActionBarTitleText</item>
    </style>

    <!-- ActionBar title text -->
    <style name="MyActionBarTitleText"
           parent="@style/TextAppearance.AppCompat.Widget.ActionBar.Title">
        <item name="android:textColor">@color/actionbar_text</item>
        <!-- The textColor property is backward compatible with the Support Library -->
    </style>

    <!-- ActionBar tabs text -->
    <style name="MyActionBarTabText"
           parent="@style/Widget.AppCompat.ActionBar.TabText">
        <item name="android:textColor">@color/actionbar_text</item>
        <!-- The textColor property is backward compatible with the Support Library -->
    </style>
</resources>
```
### 自定义Tab Indicator
重写actionBarTabStyrle指定navigation tabs
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_focused="false" android:state_selected="false"
          android:state_pressed="false"
          android:drawable="@drawable/tab_unselected" />
    <item android:state_focused="false" android:state_selected="true"
          android:state_pressed="false"
          android:drawable="@drawable/tab_selected" />
    <item android:state_focused="true" android:state_selected="false"
          android:state_pressed="false"
          android:drawable="@drawable/tab_unselected_focused" />
    <item android:state_focused="true" android:state_selected="true"
          android:state_pressed="false"
          android:drawable="@drawable/tab_selected_focused" />
    <item android:state_focused="false" android:state_selected="false"
          android:state_pressed="true"
          android:drawable="@drawable/tab_unselected_pressed" />
    <item android:state_focused="false" android:state_selected="true"
        android:state_pressed="true"
        android:drawable="@drawable/tab_selected_pressed" />
    <item android:state_focused="true" android:state_selected="false"
          android:state_pressed="true"
          android:drawable="@drawable/tab_unselected_pressed" />
    <item android:state_focused="true" android:state_selected="true"
          android:state_pressed="true"
          android:drawable="@drawable/tab_selected_pressed" />
</selector>
```
Android 3.0
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="CustomActionBarTheme"
           parent="@style/Theme.Holo">
        <item name="android:actionBarTabStyle">@style/MyActionBarTabs</item>
    </style>

    <style name="MyActionBarTabs"
           parent="@style/Widget.Holo.ActionBar.TabView">
        <!-- tab indicator -->
        <item name="android:background">@drawable/actionbar_tab_indicator</item>
    </style>
</resources>
```
Android 2.1 
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="CustomActionBarTheme"
           parent="@style/Theme.AppCompat">
        <item name="android:actionBarTabStyle">@style/MyActionBarTabs</item>

        <!-- Support library compatibility -->
        <item name="actionBarTabStyle">@style/MyActionBarTabs</item>
    </style>

    <style name="MyActionBarTabs"
           parent="@style/Widget.AppCompat.ActionBar.TabView">
        <item name="android:background">@drawable/actionbar_tab_indicator</item>

        <item name="background">@drawable/actionbar_tab_indicator</item>
    </style>
</resources>
```
## Action Bar 覆盖叠加
![](https://github.com/zt1991616/blog/raw/master/Image/14072805.png)
- 启动叠加模式
    自定义主题 设置`android:windowActionBarOverlay`的属性为`true`
```xml
<!-- android 3.0-->
<resources>
    <style name="CustomActionBarTheme"
           parent="@android:style/Theme.Holo">
        <item name="android:windowActionBarOverlay">true</item>
    </style>
</resources>
<!-- android 2.1 -->
<resources>
    <style name="CustomActionBarTheme"
           parent="@android:style/Theme.AppCompat">
        <item name="android:windowActionBarOverlay">true</item>

        <!-- Support library compatibility -->
        <item name="windowActionBarOverlay">true</item>
    </style>
</resources>
```
- 指定布局的顶部边距
```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingTop="?android:attr/actionBarSize">
    <!-- Support库中 移除android:前缀 paddingTop="?attr/actionBarSize" -->
    ...
</RelativeLayout>
```

## 添加一个Action View
action view是一个显示于action bar中的widget，用于替代某个action项按钮.

![](https://github.com/zt1991616/blog/raw/master/Image/14041501.png)

在菜单资源中声明一个action view项，使用`android:actionLayout`或`android:actionViewClass`属性指定一个layout资源或所需的widget类即可。比如：
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
	<item android:id=@"+id/menu_search"
		android:title=@"string/menu_search"
		android:icon=@"drawable/ic_menu_search"
		android:showAsAction="ifRomm|collapseActionView"
		android:actionViewClass="android.widget.SearchView" />
</menu>
```
**注意**，`android:showAsAction`属性还包含了`collapseActionView`。这是可选项，表示action view应该折叠进入按钮中，当用户选中了按钮，action view就会展开。否则，action viewm默认就是可见的。
action view可以通过下单项的ID调用`findItem()`，然后调用`getActionView()`。
```java
@Override
public boolean onCreateOptionsMenu(Menu menu){
	getMenuInflater().inflate(R.menu.options,menu);
	SearchView searchView = (SavechView)menu.findItem(R.id.menu_search).getActionView();
	// ...

	return super.onCreateOptionsMenu(menu);
}
```

