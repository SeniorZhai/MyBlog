title: Data Binding（一）
date: 2015-08-16 09:20:21
categories: Android
tags: [KVO,数据绑定,MVVM]
---
在iOS有个概念叫KVO，它提供一致机制，当制定对象的属性被修改时，会发出一条通知。这样每次被观察的对象的属性被修改后，KVO就会自动通知响应的观察者。
Google推出的Data Binding也可以帮我们实现这一点。
<!--more-->
Binding是Google提供的一个Support库，最低可以支持到Android 2.1(API 7+)。

	环境：Android Studio 1.3-beta1 以上版本

有了`Data Binding`，我们就可以使用使用`MVVM`模式构建我们的APP了。
在Android的传统模式(MVC)下，Model一般是Json数据，V就是我们的Layout文件生成的View，C作为核心的控制器，一般由Activity、Fragment出任。当时这个模式下，C的责任比较大，所以逻辑最复杂，代码量也最多(而且还有很大部分的findViewById)。
在MVVM模式下，解决了Android UI编程的一大痛点，只要建立好MVVM之间的联系，我们可以减少很大一部分的setText()、getValue()之类的代码。
## 构建环境
在项目的gradle文件中添加
```
denpendcies {
	classpath "com.android.tools.build:gradle:1.3.0"
	classpath "com.android.databinding:dataBinder:1.0rc1""
}
```
同时确保`jcenter`在`repostiories`列表里
```
allprojects {
	repositories {
		jcenter()
	}
}
```
在`module`的gradle文件中添加插件
```
apply plugin: 'com.android.application'
apply plugin: 'com.android.databinding'
```

## 基本概念
在基本的使用下，M依旧是Json为主的Java Bean对象，V还是XML为主的Layout布局，但是承当的责任会更多，VM作为中间的桥梁需要建立他们的联系，同时完成自己的本质工作(Acticity和Fragment等)

### 静态数据
首先完成Model的定义
```java
public class User {
	public String name;
    public int age;

    public StaticUser(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public boolean isAudlt (){
        return this.age >=18;
    }

    public String getName() {
        return this.name;
    }
}
```
<https://github.com/SeniorZhai/DataBindingDemo/blob/master/app/src/main/java/io/github/seniorzhai/databindingdemo/model/StaticUser.java>
然后创建View，使用Data Binding，布局的跟标签为layout
```xml 
<layout xmlns:android="http://schemas.android.com/apk/res/android">
	<!-- 定义数据 -->
    <data>
    	<!-- 导入View类 -->	
        <import type="android.view.View" />
        <!-- 导入StaticUser类 -->
        <variable
            name="user"
            type="io.github.seniorzhai.databindingdemo.model.StaticUser" />
    </data>
    <!-- 布局 -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.getName()}" />
        <!-- 默认无需导入java.lang包 -->
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{String.format(`Age : %d`,user.age)}" />
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Adult"
            android:visibility="@{user.isAudlt ? View.VISIBLE : View.GONE}" />
    </LinearLayout>
</layout>
```
<https://github.com/SeniorZhai/DataBindingDemo/blob/master/app/src/main/res/layout/activity_static.xml>
最后在Activity中建立联系
```java
	StaticUser user;
	@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 默认情况下Data Binding插件会根据layout文件的名称产生Binding类
        ActivityStaticBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_static);
        user = new StaticUser("SeniorZhai", 24);
        binding.setUser(user);
    }
```
<https://github.com/SeniorZhai/DataBindingDemo/blob/master/app/src/main/java/io/github/seniorzhai/databindingdemo/StaticActivity.java>
### 动态数据
首先需要完成我们的Model，这时候Model需要继承BaseObservable，并且通过制定一个`Bindable`注解给getter以及setter内通知实现属性改变时发出通知。
```java
public class ObservableUser extends BaseObservable {
    public String name;

    @Bindable
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
        notifyPropertyChanged(BR.name);
    }
}
```
在XML中
```xml
<data>
	<variable
            name="user"
            type="io.github.seniorzhai.databindingdemo.model.ObservableUser" />
</data>
```
<https://github.com/SeniorZhai/DataBindingDemo/blob/master/app/src/main/res/layout/activity_observale.xml>
最后在Activity中添加代码，建立联系
```java
ActivityObservaleBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_observale);
binding.setUser(user);
```
<https://github.com/SeniorZhai/DataBindingDemo/blob/master/app/src/main/java/io/github/seniorzhai/databindingdemo/ObservaleActivity.java>