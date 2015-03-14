title: androidannotations简介
date: 2015-02-28 21:30:45
categories: Android
tags: [注入]
---
[androidannotations](https://github.com/excilys/androidannotations)是一款Android注入框架，可以方便我们编程，减少代码量(变相减少了错误的可能)，让我们可以更多的把精力放在逻辑处理上。
<!--more-->
本文API介绍取至<https://github.com/excilys/androidannotations/wiki/Cookbook>
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
