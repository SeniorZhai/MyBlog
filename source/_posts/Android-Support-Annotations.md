title: Android Support Annotations
date: 2015-08-03 20:05:45
categories:
tags:
---
Android Support Library 19.1版本中加入了几个好用的注解
<!--more-->
通过gradle导入工程中
```
compile 'com.android.support:support-annotations:22.2.0'
```
有以下三种注解：
- Nullness注解
- 资源类型注解
- IntDef和StringDef注解

## Nullness注解
- 使用`@NonNull`注解修饰的参数不能为null
```java
void say(@NonNull String s){
	// ...
}

say(null); // 在IDE中，这行代码会提示警告
```
- 使用`@Nullable`注解修饰的函数参数或者返回值可以为null
```java
@Nullable
String getName(@NonNull String str){
	return str;
}
```

## 资源类型注解
- 使用`@StringRes`注解表示函数期望接受一个字符串类型的id，而不是一个普通的int
```java
void show(@StringRes int id) {
	getResources().getString(id); 
}
```
同理可用的注解还有`AnimatorRes`,`AnimRes`,`ArrayRes`,`ColorRes`,`DimenRes`,`DrawableRes`,`IdRes`,`StyleRes`

## IntDef和StringDef注解
很多时候我们用枚举类型来控制一些模式
这个时候可以使用`@IntDef`来代替
```java
public static final int VANILLA = 0;
public static final int CHOCOLATE = 1;

@InDef({VANILLA,CHOCOLATE})
public @interface Flavour{}

// 使用@Flavour自定义注解，表示它可以接受的值的类型
public void setFlavour(@Flavour int flavour) {
	
}
```