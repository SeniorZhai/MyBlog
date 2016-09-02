title: 环信SDK(一)
date: 2014-09-18 17:17:32
categories: Android
tags: [SDK,IM,环信]
---
[环信](http://easemob.com/)提供了一套IM SDK，可以很方便的在App中集成IM功能
<!--more-->
## 添加权限和App Key
```xml
<!-- 权限 -->
<uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.CALL_PHONE" />
    <uses-permission android:name="android.permission.GET_TASKS" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
<!-- App Key -->
<meta-data
    android:name="EASEMOB_APPKEY"
    android:value="xxxx# mychatdemo" />
```
## 初始化
在自定义的Application中初始化，确保应用程序一开始就得到运行
```java
public class MyApplication extends Application {
	@Override
	public void onCreate() {
		super.onCreate();
		EMChat.getInstance().init(this);

		EMChatOptions options = EMChatManager.getInstance().getChatOptions();
		options.setAcceptInvitationAlways(false);	// 设置添加好友时，默认不需要验证，改为需要验证
		options.setNotificationEnable(flase);	// 设置收到消息是否有新消息通知，默认是true
		options.setNoticeBySound(false);	// 设置接收到消息是否声音通知，默认是true
		options.setNoticedByVibrate(false);	// 设置接收到消息是否震动，默认是true
		options.setUseSpeaker(false);		// 设置语音消息播放是否为扬声器播放，默认是true
	}
}
```
## 注册
```java
String appkey = EMChatConfig.getInstance().APPKEY;
EMChatManager.getInstance().createAccountOnServer(appkey + "_" + username, pwd); //注意用户名不能有大写字母
```
## 登陆
```java
EMChatManager.getInstance().login(username,password,new EMCallBack() {
	@Override
	public void onSuccess() {

	}
	@Override
	public void onProgress(int progress,String status) {

	}
	@Override
	public void onError(int code,String message) {

	}
});
```
## 监听消息
接收聊天消息，回执消息，好友同意，好友请求等
- 聊天信息
```java
msgReceiver = new NewMessageBroadcastReceiver();
IntentFilter intentFilter = new IntentFilter(EMChatManager.getInstance().getNewMessageBroadcastReceiver());
intentFilter.setPriorty(3);
registerReceiver(msgReceiver,intentFilter);
```
- 回执消息
```java
IntentFilter ackMessageIntentFilter = new IntentFilter(EMChatManager.getInstance().getAckMessageBroadcastAction());
ackMessageIntentFilter.setPriority(3);
registerReceiver(ackMessageReceiver, ackMessageIntentFilter);
```
- 好友同意、好友请求
```java
IntentFilter inviteIntentFilter = new IntentFilter(EMChatManager.getInstance().getContactInviteEventBroadcastAction());
registerReceiver(contactInviteReceiver, inviteIntentFilter);
```
- 联系人变化
```java
EMContactManager.getInstance().setContactListener(new MyContactListener());
```
- 连接状态
```java
EMChatManager.getInstance().addConnectionListener(new MyConnectionListener());
```
## 发送消息
```java
// 创建一个消息
EMMessage msg = EMMessage.createSendMessage(EMMessage.Type.TXT);
// 设置消息的接受方
msg.setReceipt(username);
TextMessageBody body = new TextMessageBody(tvMsg.getText().toString());
msg.addBody(body);
try {
   //发送消息
   EMChatManager.getInstance().sendMessage(msg);
} catch (Exception e) {
   e.printStackTrace();
}
```
## 接收消息
```java
private class NewMessageBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        //消息id
        String msgId = intent.getStringExtra("msgid");
        ......
        ......
        ......

        //注销广播，否则在ChatActivity中会收到这个广播
        abortBroadcast();
    }
}
```
## 消息回执
```java
private BroadcastReceiver ackMessageReceiver = new BroadcastReceiver() {
	
    @Override
    public void onReceive(Context context, Intent intent) {
        //消息id
        String msgId = intent.getStringExtra("msgid");
        ......
        ......
        ......
        abortBroadcast();
	}
};
```
## 联系人变化
```java
private class MyContactListener implements EMContactListener{
     @Override
     public void onContactAdded(List<String> usernameList) {
     }
     @Override
     public void onContactDeleted(List<String> usernameList) {
     }
}
```
## 连接状态
```java
private class MyConnectionListener implements ConnectionListener{
    @Override
    public void onConnected() {
    }
    @Override
    public void onDisConnected(String errorString) {
      if(errorString!=null&&errorString.contains("conflict"))
      {
        //收到帐号在其他手机登录
        // TODO 
      }else{
        //"连接不到聊天服务器"
      }
    }
    @Override
    public void onReConnected() {
    }
    @Override
    public void onReConnecting() {
    }
    @Override
    public void onConnecting(String progress) {
    }
}
```
## 退出登陆
```java
@Override
protected void onPause() {
    super.onPause();
    //登出聊天服务器
    EMChatManager.getInstance().logout();
}
```
## 设置昵称
```java
EMChatManager.getInstance().updateCurrentUserNick(nickname);
```
## 例子
[ChatDemo](https://github.com/SeniorZhai/ChatDemo)