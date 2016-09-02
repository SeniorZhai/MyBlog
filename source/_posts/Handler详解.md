title: Handler详解
date: 2015-02-13 09:51:49
categories: Android 
tags: [Handler]
---
Handler是一个Android开发中常用的类，主要用于接收子线程发送的数据，并用此数据配合主线程更新UI，也可以传递消息。
<!--more-->
总所周知，Android中非UI线程操作UI时就会Crash，从而保存UI线程的稳定


## 用法
```java
handler.post(new Runnable() {
	@Override
	public void run(){
		textView.setText("更新UI！！！");
	}
});
```