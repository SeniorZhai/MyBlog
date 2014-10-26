title: runonUiThread()更新UI
date: 2014-10-26 18:41:35
categories: Android
tags: [线程,Thread,Ui]
---
Activity.runOnUiThread()是Handlers的特殊情况，使用Handlers可以在自己的线程里创建事件查询。
<!--more-->
默认情况下，使用handler不是"代码在UI线程中运行"，handlers会绑定到其实例化的线程上，想要创建直接绑定UI线程的handlers需要looper
```java
Handler mHandler = new Handler(Looper.getMainLooper());
```
如果使用runOnUiThread，需要使用handler：
```java
public final void runOnUiThread(Runnable action) {
	if (Thread.currentThread() != mUiThread) {
		mHandler.post(action);
	} else {
		action.run();
	}
}
```