title: LayoutInflater的inflate函数
date: 2014-06-11 13:24:20
categories: Android
tags: [Android]
---
LayoutInflater的作用是将layout的xml布局文件实例化为View类对象。
获取LayoutInflater的方法有如下三种

```java
LayoutInflater
 inflater=(LayoutInflater)context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);

View
 layout = inflater.inflate(R.layout.main, null);

 LayoutInflater
 inflater = LayoutInflater.from(context); (该方法实质就是第一种方法，可参考源代码)

 View
 layout = inflater.inflate(R.layout.main, null);

 

LayoutInflater
 inflater = getLayoutInflater();（在Activity中可以使用，实际上是View子类下window的一个函数）

View
 layout = inflater.inflate(R.layout.main, null);
```