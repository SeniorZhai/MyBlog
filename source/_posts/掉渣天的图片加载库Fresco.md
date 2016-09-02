title: 掉渣天的图片加载库Fresco
date: 2015-03-31 10:51:28
categories: Android
tags: [图片加载]
---
<!--more-->
## 开始
```gradle
dependencies {
  compile 'com.facebook.fresco:fresco:0.1.0+'
}
```
## 使用
简单的下载网络图片
- 在Application中初始化
```java
Fresco.initialize(context);
```
在XML中使用`SimpleDraweeView`，需要键入命名空间`xmlns:fresco="http://schemas.android.com/apk/res-auto"`
```xml
<com.facebook.drawee.view.SimpleDraweeView
	android:id="@+id/my_image_view"
	android:layout_width="20dp"
    android:layout_height="20dp"
    fresco:placeholderImage="@drawable/my_drawable" />
```
加载
```xml
draweeView.setImageURL("http://site.com/uri");
```
### URLs
Fresco不支持相对路径，所有URL必须是绝对路径
|类型|Scheme|
|:---|:-----|
|远程图片|http://,https://|
|本地文件|file://|
|ContentProvider|content://|
|asset目录下资源|asset://|
|res目录下资源|res://|

更多请见<http://fresco-cn.org/>