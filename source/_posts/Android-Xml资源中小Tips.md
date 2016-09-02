title: Android Xml资源中小Tips
date: 2015-04-10 16:35:37
categories: Android
tags: [style,资源]
---
<!--more-->

# @
- 引入自定义资源，格式为`@[package:]type/name`
	最常见的资源引用
```xml
android:text="@string/hello"
```

- 引入系统资源，格式为`@android:type/name`
```xml
android:textColor="@android:color/opaque_red"
```

# @*
引用系统非public资源`@*android:type/name`

> 系统资源纷纷public和非public，生命在<SDK>\platforms\android-8\data\res\values\public.xml

相比`@android:type/name`，`@*android:type/name`能够使用所有Android系统资源，不过Google官方不推介使用

# ?
`?[namespace:]type/name`这种方式用于引用当前主题中的属性值，这个属性值只能在style资源和XML属性中使用
```xml
android:textColor="?android:textDisabledColor"
```

# @+
`@+type/name`，`+`表示在type中添加一条记录通常用于定义资源`id`
```xml
android:id="@+id/select"
```