title: EventBus
date: 2014-11-08 21:30:52
categories:
tags:
---
EventBus是Android上一个事件发送/接受的解决方案之一
- 简化了组建之间的通信
	- 事件有发送方和接收方
	- 执行在Activity、Fragment和后台进程上
	- 避免了复杂的、容易出错的依赖和生命周期问题
- 快速、轻便、稳定
- 拥有先进的功能，如交付线程、用户优先级
<!--more-->
##3步使用EventBus
1. 定义事件Evenets
`public class MessageEvent{ /* 加入需要的额外字段 */ }`
2. 准备接受
`eventBus.register(this);`
`public void onEvent(AnyEventType event){ /* Do something */}`
3. 发送事件
`eventBus.post(event);`
##在工程中加入EventBus
Gradle:
	compile 'de.greenrobot:eventbus:2.2.1'
Maven:
	<dependency>
		<groupId>de.greenrobot</groupId>
		<artifactId>eventbus</artifactId>
		<version>2.2.1</version>
	</dependency>
	