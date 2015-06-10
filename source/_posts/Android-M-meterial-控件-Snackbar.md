title: Android M meterial 控件 - Snackbar
date: 2015-06-04 18:23:32
categories: Android
tags: [Android,Meterial]
---
Android M发布的Android Design Support Library提供了很多Material Design控件，支持Android 2.1及以后的版本。
再也不用担心第三方的Material Design不可靠了。
<!--more-->
##Snackbar
Snackbar类型于Toast用作轻量级的操作反馈，与[Crouton](https://github.com/keyboardsurfer/Crouton)相似(Snackbar显示在父视图的底部)，但是功能要简单的多。
- 关于`Crouton`可以查看<http://seniorzhai.github.io/2014/12/15/Crouton%E7%9A%84%E4%BD%BF%E7%94%A8/>
<p>
<video width="100%" height="240" controls=""><source src="http://ac-lhzo7z96.clouddn.com/1433285265672" type="video/mp4"></video>
</p>
```java
Snackbar
	.make(parentLayout, R.string.snackbar_text, Snackbar.LENGTH_LONG)	// 显示的文本及显示的位置
	.setAction(R.string.snackbar_action, myOnClickListener)	// 自定义点击时间
	.show(); 
```

<https://github.com/SeniorZhai/SnackbarDemo>