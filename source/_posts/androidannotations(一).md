title: androidannotations(一)
date: 2015-02-28 21:30:45
categories: Android
tags: [注入]
---
[androidannotations](https://github.com/excilys/androidannotations)是一款Android注入框架，可以方便我们编程，减少代码量(变相减少了错误的可能)，让我们可以更多的把精力放在逻辑处理上。
<!--more-->
本文API介绍取至<https://github.com/excilys/androidannotations/wiki/Cookbook>
##特征
- 依赖注入
- 简单的后台任务模型
- 事件绑定
- REST

##配置
在AS中，需要在项目的`build.gradle`中进行加入如下配置（有注释部分）
```
apply plugin: 'com.android.application'
apply plugin: 'android-apt'

buildscript {
	repositories {
		mavenCentral()
	}
	dependencies {
 		classpath 'com.android.tools.build:gradle:1.0.0'
 		// 使用android-apt
        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.4'
	}
}

repositories {
    mavenCentral()
    mavenLocal()
}

dependencies {
	def AAVersion = '3.2'
	// 设置版本
	apt "org.androidannotations:androidannotations:$AAVersion" 
	compile "org.androidannotations:androidannotations-api:$AAVersion"
}

apt {
    arguments {
        androidManifestFile variant.outputs[0].processResources.manifestFile
        resourcePackageName 'com.myproject.name'	// 指定包名
	}
}
```
##使用
- 基本使用示例
```java
@EActivity(R.layout.translate)	// 设置布局文件
public class TranslateActivity extends Activity {
	@ViewById	// 注入 等同于 findViewById(R.id.textInput)
	EditText textInput;

	@ViewById(R.id.myTextView)
	TextView result;

	@AnimationRes	// 获取 anroid.R.anim.fade_in
	Animation fadeIn;

	@Click	// 设置R.id.doTranslate的监听
	void doTranslate() {
		translateInBackground(textInput.getText().toString());
    }

    @Background // 后台线程
    void translateInBackground(String textToTranslate) {
        String translatedText = callGoogleTranslate(textToTranslate);
        showResult(translatedText);
    }

    @UiThread // UI线程
    void showResult(String translatedText) {
        result.setText(translatedText);
        result.startAnimation(fadeIn);
    }

}
```
###Activity
- `@EActivity`注入Activity
```java
@EActivity(R.layout.main)
public class MyActivity extends Activity {
	
}
```
也可以不注入
```java
@EActivity
public class MyListActivity extends ListActivity {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
	}
}
```
	
	注：使用Androidannotations注入的Activity在AndroidManifest.xml中配置时，名字后需要加上_，如MainActivity则为.MainActivity_

###Fragment
- `@EFragment`
```java
@EFragment
public class MyFragment extends Fragment {
	
}
```
在布局中需要使用`MyFragment_`表示
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="horizontal" >

    <fragment
        android:id="@+id/myFragment"
        android:name="com.company.MyFragment_"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />

</LinearLayout>
```
在程序中使用
```java
MyFragment fragment = new MyFragment_();
```
也可以注入布局
```java
@EFragment(R.layout.my_custom_layout)
public class MyFragment extends ListFragment {
  // R.layout.my_custom_layout will be injected
}
```
获取Fragment时
```java
@EActivity(R.layout.fragments)
public class MyFragmentActivity extends FragmentActivity {
  @FragmentById
  MyFragment myFragment;

  @FragmentById(R.id.myFragment)
  MyFragment myFragment2;

  @FragmentByTag
  MyFragment myFragmentTag;

  @FragmentByTag("myFragmentTag")
  MyFragment myFragmentTag2;
}
```

###自定义控件
```java
@EView
public class CustomButton extends Button {
  @App
  MyApplication application;

  @StringRes
  public CustomButton(Context context,AttributeSet attrs) {
    super.(context,attrs);
  }
}
```
在布局中使用时
```xml
<!-- ... -->
<com.myapp.view.CustomButton_
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  />
```
###自定义ViewGroups
layout
```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android" >

    <ImageView
        android:id="@+id/image"
        android:layout_alignParentRight="true"
        android:layout_alignBottom="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/check" />

    <TextView
        android:id="@+id/title"
        android:layout_toLeftOf="@+id/image"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textColor="@android:color/white"
        android:textSize="12pt" />

    <TextView
        android:id="@+id/subtitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/title"
        android:textColor="#FFdedede"
        android:textSize="10pt" />

</merge>
```
Java代码
```java
@EViewGroup(R.layout.title_with_subtitle)
public class TitleWithSubtitle extends RelativeLayout {
  @ViewById
  protected TextView title,subtitle;

  public TitleWithSubtitle(Context context,AttributeSet attrs) {
    super(context,attrs);
  }

  public void setTexts(String titleText,String subTitleText) {
    title.setText(titleText);
    subtitle.setText(subTitleText);
  }
}
```
使用
```xml
<com.myapp.viewgroup.TitleWithSubtitle_
  android:id="@+id/firstTitle"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  />
```
###Service
```java
@EService
public class MyService extends IntentService {
  @SystemService
  NotificationManager notificationManager;
  
  @Bean 
  MyEnhancedDatastore datastore;

  @RestService
  MyRestClient = myRestClient;

  @OrmLiteDao(helper = DatabaseHelper.class,model = User.class)
  UserDao userDao;

  public MyService() {
    super(MyService.class.getSimpleName);
  }

  @Override
  proteced void onHandleIntent(Intent intent){
    showTast();
  }

  @UiThread
  void showToast() {
    Toast.makeText(getApplicationContext(),"Hi~",Toast.LENGTH_LONG).show();
  }
}
```
对于IntentService还可以设置一些操作
```java
@ServiceAction
void mySimpleAction(String param) {
  //...
}
```
通过`MyIntentService_.intent(getApplication()).myAction("test").start()`使用

##获取资源
```java
// String
@StringRes(R.string.hello)
String myHelloString;
@StringRes
String hello;
// Color
@ColorRes(R.color.backgroundColor)
int someColor;
@ColorRes
int backgoundColor;
// animation
@AnimationRes(R.anim.fadein)
Animation fadein;
@AnimationRes
Animation fadein;
// DimensionRes
@DimensionRes(R.dimen.fontsize)
float fontSizeDimension;
@DimensionRes
float fontsize;
// 换算成PX
@DimensionPixelOffsetRes(R.sting.fontsize)
int fontSizeDimension;
@DimensionPixelOffsetRes
int fontsize;
```
其他
- @BooleanRes
- @ColorStateListRes
- @DrawableRes
- @IntArrayRes
- @IntegerRes
- @LayoutRes
- @MovieRes
- @TextRes
- @TextArrayRes
- @StringArrayRes

###Intent
```java
@EActivity 
public class MyActivity extends Activity {
  @Extra("myStringExtra") // 也可以胜利
  String myMessage;
  @Extra("myDateExtra")
  Date myDate = new Date();

  @Override
  protected void onNewIntent(Intent intent) {
    // 再次注入时
  }

  @AfterExtras
  public void doSomethingAfterExtrasInjection() {
    //
  }
}
```