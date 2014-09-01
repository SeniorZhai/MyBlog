title: iOS应用生命周期
date: 2014-02-26 12:43:11
categories: iOS
tags: [iOS]
---
##iOS应用状态
- Not running(未运行):应用程序`未启动`或者应用被系统`终止`
- Inactive(不活动):程序在前台运行，但不能接受事件处理。当应用从一个状态切换到另一个状态时，中途过渡会短暂停留在此状态。
- Active(活动):程序在前台运行且能接收到事件，这是应用在前台运行时所处的正常状态。
- Background(后台):应用处在后台运行，并且还在执行代码。大多数将要进入Suspended做状态的应用，会先短暂进入状态。如果一个应用要求启动时直接进入后台运行，这样应用会直接从Not running状态进入Background状态，中途不会经过Inactive状态。
- Suspended(挂起):应用处在后台，并且没有执行任何代码。系统会自动将应用转入该状态，并且不会发出任何通知。当处在该状态时，应用依然驻留内存，但不执行任何程序代码。
##App Delegate对应的回调方法
- application:willFinishLaunchingWithOptions:程序将要启动时自动调用该方法，该方法是应用程序启动时第一次执行自定义代码的机会。
- application:didFinishLaunchingWithOptions:应用程序启动时自动调用该方法，开发者可以在该方法中执行初始化相关的代码。
- applicationDidBecomeActive:应用在转入前台，并进入活动状态时回调该方法（当应用从启动到进入前台，或从后台转入前台都会调用该方法），可重写该方法执行最后的准备工作。
- applicationWillResignActive:应用正要从前台运行状态离开时将会调用该方法。
- applicationDidEnterBackground:应用在正处于Background状态，且随时可能进入Suspended状态时将会调用该方法。
- applicationWillEnterForeground:应用正从后台转入前台运行状态，但暂时还没有到达Active状态时将会调用该方法。
- applicationWillTerminate:该应用程序即将被终止时调用该方法，如果应用当前处在Suspended状态，此方法将不会被调用。