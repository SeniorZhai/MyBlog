title: BroadCastReceiver
date: 2014-09-16 15:22:30
categories: Android
tags: [Broadcast]
---
在Android中，Broadcast是一种广泛运用在应用程序之间传输信息的机制，而BroadcastReceiver是对发送出来的Broadcast进行过滤接收并响应的一类组件
<!--more-->
##注册BroadcastReceiver
在AndroidManifest.xml中用标签注册
- 静态注册过滤器
```java
<receiver android:name="myRecevice">
	<intent-filer>
		<action android:name="com.zoe.net"/>
	</intent-filer>
</receiver>
```
- 动态注册过滤器
```java
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction(String);
registerReceiver(BroadcastReceiver,intentFilter); // 在BroadCastReceiver的onStart中注册，onStop中unregiterReceiver
```

##触发BroadcastReceiver
```java
Intent intent = new Intent("com.zoe.net");
intent.putExtra("msg","发送消息");
Content.sendBroadcast(intent);
```
接受到广播消息后会执行`onReceive()`方法
```java
@Override 
public void onReceive(Context context,Intent intent) {
	String msg = intent.getStringExtra("msg");
	int id = intent.getIntExtra("who",0);
	if(intent.getAction().equals("com.zoe.sendMsg")) {
		mn = (NotificationManage)context.getSystemService(Context.NOTIFICATION_SERViCE);
		notification = new Notification(R.drawable.icon,id+"发送广播",System.currentTimeMillis());
		Intent it = new Intent(context,Main.class);
		PendIntent contentIntent = PenIntent.getActivity(context,0,it,0);
		notification.sendLatesEventInfo(context,"msg","msg",contentIntent);
		mn.notify(0,notification);
	}
}
```