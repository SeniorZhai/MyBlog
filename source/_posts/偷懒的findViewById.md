title: 偷懒的findViewById
date: 2015-01-07 17:36:29
categories: Android
tags: [findViewById]
---
<!--more-->
```java
// 在Activity的基类中定义
protected <T extends View> T $ (int id) {
	return (T) findViewById(id);
} 
// 使用很简单
bn = $(R.id.bn1);
```