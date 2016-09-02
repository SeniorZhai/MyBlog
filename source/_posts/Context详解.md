title: Context详解
date: 2014-12-23 10:31:24
categories: Android
tags: [Context]
---
Context可能是Android中最常用的元素之一，但也是最容易用错的。
Context对象常见的功能，有传递引用、加载资源、启动Activity、获取系统服务、获取内部文件路径以及创建View，但是Context的功能远不止这些。
<!--more-->
## Context的类型
Android应用中不同的组件Context对象会有细微的差别

- Application Android应用进程中的打你，在`Activity`和`Service`中，可以通过`getApplication()`函数获得，或者任何继承于`context`对象中，都可以通过`getApplicationContext()`方法获得。不管在哪里通过何种方法，在同一个进程中，获得的都是同一个实例
- Activity/Service 继承于`ContextWrapper`，实现了context同样的API，但是这些代理这些方法调用到内部隐藏的context实例。任何时候当系统创建一个新的`Activity`或者`Service`实例的时候，也创建一个新的`ContextImp`实例来处理所有繁重的工作，每个`Activity`和`Service`以及对应的基础`Context`，对于每个实例都是唯一的
- BroadcastReciver 它本身不是`Context`，也没有`Context`在它里面，但是每当一个新的广播到达的时候，框架都会传递一个`Context`对象到`onReceive()`，这个`context`是一个`ReceiveRestrictedContext`实例，它主要的两个函数被禁用了`registerReceiver()`和`binSerbice()`。每个`Receiver`处理一个广播，传递进来的`context`都是一个新的实例
- ContentProvider 它本身也不是一个`Context`，但是可以通过`getContext()`函数获取`Context`对象。如果`ContextProvider`是同一个应用进程，`getContext()`将返回`Application`单例。如果是不同进程的时候，它将返回一个新的实例代表这个`Provider`所运行的包

## 保存引用
一个对象或者类内部保存一个`Context`引用，而它的生命周期却远超过其保存引用对象的生命周期，例如创建一个自定义的单例，它需要context来加载资源或者获取ContentPrivide，从而保存一个指向当前Activity或者Service的引用

**错误的示例**
```java
public class CustomManager {
	private static CustomManger sInstance;

	public static CustomManager getInstance(Context context){
		if (sInstance == null){
			sInstance = new CustomManager(context);
		}
		return sInstance;
	}

	private Context mContext;

	private CustomManager(Context context) {
		mContext = context;
	}

	//...
}
```
**那么问题来了**————我们不知道这个Context哪里来的，并且如果保存一个最终指向的是`Activity`或者`Service`的引用是并不安全的，那么因为单例在类的内部维持一个静态的引用，这意味着我们的对象以及所有其他引用的对象将永远不能被垃圾回收。如果Context是一个Activity，那么大量的View以及其他对象不能释放，从而造成内存泄露。

解决这个问题的方法也很简单，单例永远只保留`Application context`:
```java
public class CustomManager {
	private static CustomManager sInstance;

	public static CustomManager getInstance(Context context) {
		if(sInstance == null) {
			sInstance = new CustomManager(context.getApplicationContext());
		}
		return sInstance;
	}

	private Context mContext;

	priavte CustomManger(Context context) {
		mContext = context;
	}
}
```
因为Application Context本身就是一个单例，所以我们的单例不会带来任何的内存泄露。另外一个解决方案在后台线程或者一个等待的`Handler`中保存Context的引用

## 不同的Context
来源于不同地方的context功能有略微的不同

|功能|Application|Activity|Service|ContentProvider|BroadcastReceiver|
|:---|---|----|----|----|----|
|显示Dialog|NO|YES|NO|NO|NO|
|启动Activity|NO<sub>1</sub>|YES|NO<sub>1</sub>|NO<sub>1</sub>|NO<sub>1</sub>|
|Layout Inflation|NO<sub>2</sub>|YES|NO<sub>2</sub>|NO<sub>2</sub>|NO<sub>2</sub>|
|启动Service|YES|YES|YES|YES|YES|
|绑定Service|YES|YES|YES|YES|NO|
|发送BroadcastReceive|YES|YES|YES|YES|NO<sub>3</sub>|
|加载Resource|YES|YES|YES|YES|YES|YES|YES|

	注：
	1. NO<sub>1</sub>Application可以开启一个Activity，但是会创建一个新的task，形成一个不标准的回退栈（back stack）
	2. NO<sub>2</sub>可以使用，但是会才用系统默认的Theme，而不是自定义的Theme
	3. NO<sub>3</sub>在Android 4.2以上，如果Receiver是null的话（这是用来获取一个sticky broadcast的当前值），这是操作是允许的

### UI
一般情况下，所有UI相关的任务都应该交给Activity处理
Application context创建Dialog或者开启Activity，系统会抛出一个异常，不允许这么做
但是大多数时候创建Layout inflation是允许被创建的，且不报错，但是主题会是系统默认的。这是因为Activity是唯一绑定在manifast文件中定义主题的Context。其他的Context实例都将使用系统默认的主题来inflater你的View。






