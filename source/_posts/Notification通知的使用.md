title: Notification通知的使用
date: 2014-10-31 23:49:55
categories:
tags:
---
<!--more-->
Notification是应用程序提醒用户发生某些事件的一种方式，包括以下功能：
1. 显示状态栏图标
2. 灯光/LED闪烁
3. 让手机振动
4. 发出声音提醒（铃声、Media Store中的音频）
5. 在通知托盘中显示额外的信息
6. 在通知栏中使用交互式操作来广播Intent

##Notification Manager
`NotificationManager`是用来处理Notification的系统Service，使用getSystemService方法可以获得对它的引用
```java
NotificationManager notificationManager = (NotificationManager)gerSystemService(Context.NOTIFICATION_SERVICE);
```
通过`NotificationManager`可以触发新的Notification，修改现有的Notification删除不需要的Notifition

### 创建Notification
向用户传递信息的方式：
1. 状态栏图标
2. 声音、闪光灯和振动
3. 在展开的通知托盘中显示详细信息

####创建Notification和配置状态栏图标
```java
Notification notification = new Notification(R.drawable.icon,"Notification",System.currentTimeMills());	// 指定图标，文本，时间戳
```
还可以设置Notification对象的number属性来显示一个状态栏显示一个状态图标所表示的时间数量

####使用默认的Notification声音、闪灯和振动
向Notification添加声音、闪灯和振动效果的可以使用默认的设置：
- Notification.DEFAULT_LIGHTS 灯
- Notification.DEFAULT_SOUND 声音
- Notification.DEFAULT_VIBRATE 振动
```java
notification.defaults = Notification.DEFAULT_SOUND | Notification.DEFAULT_VIBRATE;
```
可以使用`Notification.DEFAULT_ALL`开启全部

####发出声音
使用sound属性向Notification分配一个新的声音并指定音频文件的URI
```java
Uri ringURI = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
notification.sound = ringURI;
```

####设备振动
首先需要设置权限
```xml
<user-permission android:name="android.permisson.VIBRATE" />
```
设置振动方式时，可以向Notification的vibrate属性分配一个long[]类型数组来设置振动和暂停时间的交替
```java
notification.vibrate = new long[] {1000,1000,1000,1000}
```

#### 闪屏
Notification还可以设置设备的LED的颜色和闪烁频率
```java
notification.ledARGB = Color.RED;	//设置LED颜色
notification.ledOffMS = 0;	//设置LED闪烁的频率
notification.ledOnMS = 1;	
notification.flags = notification.flags | Notification.FLAG_SHOW_FIGHTS;	// 要使用LED必须添加flags属性
```

####Notification Builder
Api 11(Android 3.0)引入的Api，简化了Notification的配置
```java
Notification.Builder builder = new Notification.Builder(mContext);
builer.setSmallIcon(R.drawable.icon);
	.setTicker("Notification")
	.setWhen(System.currrntTimeMillis())
	.setDefaults(Notification.DEFAULT_SOND | 
			Notification.DEFAULT_VIBRATE)
	.setSound(
			RingtoneManager.getDefaultUri(
				RingtoneManager.TYPE_NOTIFICATION))
	.setVibrate(new long[] {1000,1000,1000,1000,1000,1000})
	.setLights(Color.RED,0,1);

Notication notification = builder.getNotification();
```

###设置和自定义通知托盘UI
配置Notification的外观主要方法有
- 设置setLatesEventInfo方法更新标准的通知托盘所显示的详细信息
- 设置Notification Builder创建和控制众多可选通知托盘UI中的一个
- 设置contentView和contentIntent属性，以便使用Remote View对象展开的状态显示分配一个自定义的UI
- 从Api 11(Android 3.0)开始，描述自定义UI的Remote Views对象可以为每个View分配一个Broadcast Intent，以使他们完全可交互

####标准的Notification UI
```