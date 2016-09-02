title: Android开发函数库caffeine
date: 2015-03-26 10:36:10
categories: Android
tags: [常用,基础,函数]
---
相信很多Android开发都有自己的函数库，来完成一些常用的操作，比如dpi操作、获取手机信息、文件操作等。[caffeine](https://github.com/percolate/caffeine)是一个Android常用函数库，封装了大量常用但是冗长的函数。
<!--more-->
## 使用
- 下载[jar](https://github.com/percolate/caffeine/tree/master/distribution)
- gradle `compile 'com.percolate:caffeine:0.3.3'`

## 常用方法
详情请见<http://percolate.github.io/caffeine/javadoc/>

### ActivityUtils
-  launchActivity
	public static void launchActivity(android.app.Activity context,
		java.lang.Class<? extends android.app.Activity> activity,
		boolean closeCurrentActivity,
		java.util.Map<java.langString,java.lang.String> params)

	+ context 为上下文
	+ activity 进入的activity
	+ closeCurrentActivity 是否finish掉当前的Activity
	+ params 通过Bundle传递的参数

	重载的方法有：`launchActivity(android.app.Activity, Class, boolean, java.util.Map)`

- getExtraString
	public static java.lang.String getExtraString(android.app.Activity context,java.lang.String key)
	获取通过Bundle传入的参数

- getExtraObject
	public static java.lang.Object getExtraObject(android.app.Activity context,java.lang.String key)
	获取传入的对象

- turnScreenOn
	public static void turnScreenOn(android.app.Activity context)
	点亮屏幕

### MiscUtils
- dpToPx
	public static int dpToPx(android.content.Context context,int dp)
	dp转px

- getVersionName
	public static java.lang.String getVersion(android.content.Context context)
	获取versionName

- getVerionCode
	public static int getVersionCode(android.content.Context context)
	获取versionCode

- getApplicationName
	public static java.lang.String getApplicationName(android.content.Context context)
	获取application的名字

- isValidEmail
	public static final boolean isValidEmail(java.lang.String email)
	判断email是否符合格式

- isVlidPhoneNumber
	public static final boolean isValidPhoneNumber(java.lang.String number)
	判断是否符合电话号码

- isValidURL
	public static final boolean isValidURL(java.lang.String url)
	判断是符合WEB_URL

### PhoneUtils
- isRotationEnable
	public static boolean isRotationEnabled(android.content.Context context)
	检查用户是否关闭了自动旋转

- isNetworkAvailable
	public static boolean isNetworkAvailable(android.content.Context context)
	判断网络是否可用

- isConnectedWifi
	public static boolean isConnectedWifi(android.content.Context context)
	判断是否连接Wifi

- isConnectedMobile
	public static boolean isConnectedMobile(android.content.Context context)
	判断是否连接手机网络




