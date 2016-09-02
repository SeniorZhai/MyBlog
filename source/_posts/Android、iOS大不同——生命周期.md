title: Android、iOS大不同——生命周期
date: 2014-12-11 10:07:29
categories: [iOS]
tags: [Android,iOS,Java,Objective-C,大不同,生命周期]
---
<!--more-->
## iOS Application生命周期
### AppDelegate
![](/img/iOS_LifeCycle.png)
1. `- (BOOL)application:(UIApplication *)applicationwillFinishLaunchingWithOptions:(NSDictionary *)launchOptions`
	将要启动
2. `- (BOOL)application:(UIApplication *)applicationdidFinishLaunchingWithOptions:(NSDictionary *)launchOptions`
	程序首次完成启动时执行，若直接启动，launchOptions没有数据，若由其他应用启动，launchOptions包含数据
3. `- (void)applicationWillResignActive:(UIApplication *)application`
	应用进入后台，程序失去激活(Active)时调用
	需要在此方法中执行下列任务
	- 暂停正在执行的任务
	- 禁止计时器
	- 减少OpenGL ES帧率
	- 若为游戏应暂停游戏
4. `- (void)applicationDidEnterBackground:(UIApplication *)application`
	已经进入后台时调用
	- 释放共享资源
	- 保存用户数据
	- 作废计时器
	- 保存足够的程序状态以便一次恢复
5. `- (void)applicationWillEnterForeground:(UIApplication *)application`
	从后台进入前台时调用
6. `- (void)applicationDidBecomeActive:(UIApplication *)application`
	应用进入激活状态
7. `- (void)applicationWillTerminate:(UIApplication *)application`
	应用兼将要退出时调用
### UIController
![](/img/iOS_LifeCycle_ViewController.png)
- alloc 创建对象
- init 初始化对象
- loadView 从nib载入视图
- viewDidLoad 载入完成
- viewWillApper 视图出现在屏幕之前
- viewWillDisapper 渲染完成，显示在屏幕上
- viewDiddisapper 从屏幕移除之前
- viewWillUnload 已经从屏幕移除
- viewDidUnload 载出之前 
- dealloc 被销毁
## Android Activity生命周期
![](/img/Android_LifeCycle.png)
1. `onCreate()`
	创建Activity
2. `onStart()`
	创建或从后台重新到前台时被调用
3. `onRestart()`
	从后台到前台时被调用
4. `onResume()`
	创建或者被覆盖、后台重新回到前台时被调用
5. `onPause()`
	被覆盖或者锁屏时被调用
6. `onStop()`
	退出当前Activity或者跳转到新的Activity时被调用
7. `onDestroy()`
	退出Activity时被调用，调用之后Activity就被销毁了
8. `onSaveInstanceState(Budle outState)`
	Activity被系统销毁时调用
9. `onRestoreInstanceState(Budle savedInstanceState)`
	Activity重建时被调用
## 对比
### 第一次启动
iOS:
```objective-c
- [AppDelegate application:didFinishLaunchingWithOptions:]
- [ViewController viewDidLoad]
- [ViewController viewWillAppear:]
- [AppDelegate applicationDidBecomeActivie:]
- [ViewController viewDidAppear:]
```
Android:
```java
onCreate()
onStart()
onResume()
```
### 应用进入后台
iOS
```objective-c
- [AppDelegate applicationWillResignActive:]
- [AppDelegate applicationDidEnterBackground:]
```
Android
```java
onPause()
onStop()
```
### 应用从后台进入前台
iOS
```objective-c
- [AppDelegate applicationWillEnterForeground:]
- [AppDelegate applicationDidBecomeActive:]
```
Android
```java
onStart()
onResume()
```
### 完全退出应用
iOS
```objective-c
- [AppDelegate applicationDidEnterBackground:]
- [ViewController viewWillDisappear:]
- [ViewController viewDidDisappear:]
- [AppDelegate applicationWillTerminate:]
```
Android
```java
onPause()
onStop()
onDestroy()
```
## 分析
Android、iOS的生命周期看着大同小异，但是差别还算比较大。
Android的Activity类似iOS中UIApplication + UIViewController。
iOS的应用像是一个全屏展开的窗口，UIApplication负责管理运行状态的生命周期，UIController负责管理视图，视图间靠通知传递数据
Android的App可以理解为`Activity`，`Service`，`Centent Provider`，`BroadcastReceiver`组成，可视部分主要由Activity组成，Activity要管理运行状态和视图，而且每个Activity的运行状态相对独立，四大组件之间、包括应用之间都可以通过`Intent`传递数据。