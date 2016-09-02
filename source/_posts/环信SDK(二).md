title: 环信SDK(二)
date: 2014-09-19 09:39:50
categories: Android
tags: [SDK,IM,环信]
---
环信单聊API
<!--more-->
## 登陆
```java
EMChatManager.getInstance().login(userName,password,new EMCallBack(){
	@Override
	public void onSuccess() {
		runOnUiTread(new Runnable(){
			public void run(){
				// 成功
			}
		});
	}
	@Override
	public void onProgress(int progress,String status) {

	}
	@Override
	public void onError(int code,String message) {
		// 失败
	}
});
```
## 注销
```java
EMChatManager.getInstance().login();
```
## 发送文本消息
```java
// 获取会话
EMConversation conversation = EMChatManager.getInstance().getConversation(username);
// 创建文本消息
EMMessage message = EMMessage.createSendMessage(EMMessage.Type.TXT);
// 设置为群聊聊
message.setChatType(ChatType.GroupChat);
// 设置消息
TextMessageBody txtBody = new TextMessageBody(content);
message.addBody(txtBody);
// 设置接收人
message.setReceipt(username);
// 把消息加入到会话中
conversation.addMessage(message);
// 发送消息
EMChatManager.getInstance().sendMessage(message,new EMCallBack());
```
## 发送语音消息
```java
EMConversation conversation = EMChatManager.getInstance().getConversation(username);
EMMessage message = EMMessage.createSendMessage(EMMessage.Type.VOICE);
// 设置为群聊
VoiceMessageBody body = new VoiceMessageBody(new File(filePath), len);
message.addBody(body);
message.setReceipt(username);
conversation.addMessage(message);
EMChatManager.getInstance().sendMessage(message, new EMCallBack());
```
## 发送图片消息
```java
EMConversation conversation = EMChatManager.getInstance().getConversation(username);
EMMessage message = EMMessage.createSendMessage(EMMessage.Type.IMAGE);
//如果是群聊，设置chattype,默认是单聊
message.setChatType(ChatType.GroupChat);
ImageMessageBody body = new ImageMessageBody(new File(filePath));
// 默认超过100k的图片会压缩后发给对方，可以设置成发送原图
// body.setSendOriginalImage(true);
message.addBody(body);
message.setReceipt(username);
conversation.addMessage(message);
EMChatManager.getInstance().sendMessage(message, new EMCallBack());
```
## 发送地理位置消息
```java
EMConversation conversation = EMChatManager.getInstance().getConversation(username);
EMMessage message = EMMessage.createSendMessage(EMMessage.Type.LOCATION);
//如果是群聊，设置chattype,默认是单聊
message.setChatType(ChatType.GroupChat);
LocationMessageBody locBody = new LocationMessageBody(locationAddress, latitude, longitude);
message.addBody(locBody);
message.setReceipt(username);
conversation.addMessage(message);
EMChatManager.getInstance().sendMessage(message, new EMCallBack());
```
## 发送文件消息
```java
EMConversation conversation = EMChatManager.getInstance().getConversation(username);
// 创建一个文件消息
EMMessage message = EMMessage.createSendMessage(EMMessage.Type.FILE);
// 如果是群聊，设置chattype,默认是单聊
if (chatType == CHATTYPE_GROUP)
	message.setChatType(ChatType.GroupChat);
//设置接收人的username
message.setReceipt(username);
// add message body
NormalFileMessageBody body = new NormalFileMessageBody(new File(filePath));
message.addBody(body);
conversation.addMessage(message);
EMChatManager.getInstance().sendMessage(message, new EMCallBack());
```
## 接收消息
```java
NewMessageBroadcastReceiver msgReceiver = new NewMessageBroadcastReceiver();
IntentFilter intentFilter = new IntentFilter(EMChatManager.getInstance().getNewMessageBroadcastAction());
intentFilter.setPriority(3);
registerReceiver(msgReceiver, intentFilter);

private class NewMessageBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        //消息id
        String msgId = intent.getStringExtra("msgid");
        //发消息的人的username(userid)
        String msgFrom = intent.getStringExtra("from");
        //消息类型，文本，图片，语音消息等,这里返回的值为msg.type.ordinal()。
        //所以消息type实际为是enum类型
        int msgType = intent.getIntExtra("type", 0);
        //消息body，为一个json字符串
        String msgBody = intent.getStringExtra("body");
        Log.d("main", "new message id:" + msgId + " from:" + msgFrom + " type:" + msgType + " body:" + msgBody);
      
        //更方便的方法是通过msgId直接获取整个message
        EMMessage message = EMChatManager.getInstance().getMessage(msgId);
                
        }
}
```
## 获取聊天记录
```java
EMConversation conversation = EMChatManager.getInstance().getConversation(username);
//获取此会话的所有消息
List<EMMessage> messages = conversation.getAllMessages();
//sdk初始化加载的聊天记录为20条，到顶时需要去db里获取更多
//获取startMsgId之前的pagesize条消息，此方法获取的messages sdk会自动存入到此会话中，app中无需再次把获取到的messages添加到会话中
List<EMMessage> messages = conversation.loadMoreMsgFromDB(startMsgId, pagesize);
//如果是群聊，调用下面此方法
List<EMMessage> messages = conversation.loadMoreGroupMsgFromDB(startMsgId, pagesize);
```
## 获取未读消息数量
```java
EMConversation conversation = EMChatManager.getInstance().getConversation(username);
conversation.getUnreadMsgCount();
```
## 未读消息清零
```java
EMConversation conversation = EMChatManager.getInstance().getConversation(username);
conversation.resetUnsetMsgCount();
```
## 清空会话记录
```java
//清空和某个user的聊天记录，不删除整个会话
EMChatManager.getInstance().clearConversation(username);
```
## 删除聊天记录
```java
//删除和某个user的整个的聊天记录
EMChatManager.getInstance().deleteConversation(username);
//删除当前会话的某条聊天记录
EMConversation conversation = EMChatManager.getInstance().getConversation(username);
conversation.removeMessage(deleteMsg.msgId);
```
## 设置自定义的消息提示
App在后台时，新消息会通过notification提示，可以把提示的内容换成自定义的内容，在Application中的onCreate()里设置
```java
// 获取到options对象
EMChatOptions options = EMChatManager.getInstance().getChatOptions();
// 设置自定义的文字提醒
options.setNotifyText(new OnMessageNotifyListener() {
	@Override
	public String onNewMessageNotify(EMMessage message){
		return message.getFrom() + "发来了一条消息";
	}
	@Override
	public String onLatestMessageNotify(EMMessage message,int fromUserNum,int messageNum) {
		return fromUserNum + "个好友发来了" + messgaeNum + "条消息";
	}
});
```
## 设置notification点击跳转的intent
```java
EMChatOptions options = EMChatManager.getInstance().getChatOptions();
options.setOnNotificationClickListener(new OnNotificationClickListener(){
	@Override
	public Intent onNotificationClick(EMMessage message) {
		Intent intent = new Intent(applicationContext,ChatActivity.class);
		ChatType chatType = message.getChatType();
		if(chatType == ChatType.chat){
			intent.putExtra("userId", message.getFrom());
			intent.putExtra("chatType", ChatActivity.CHATTYPE_SINGLE);
		} else {
			//message.getTo()为群聊id
			intent.putExtra("groupId", message.getTo());
			intent.putExtra("chatType", ChatActivity.CHATTYPE_GROUP);
		}
		return intent;
	}
});
```
## 好友列表
```java
List<String> usernames = EMChatManager.getInstance().getContactUesrNames();
```
## 添加好友
```java
//参数为要添加的好友的username和添加理由
EMContactManager.getInstance().addContact(toAddUsername, reason);
```
## 删除好友
```java
EMContactManager.getInstance().deleteContact(username);
```
## 同意好友请求
```java
//同意username的好友请求
EMChatManager.getInstance().acceptInvitation(username);
```
## 拒绝好友请求
```java
EMChatManager.getInstance().refuseInvitation(username);
```
## 监听好友请求
```java
EMContactManager.getInstance().setContactListener(new EMContactListener() {
	
	@Override
	public void onContactAgreed(String username) {
		//好友请求被同意
	}
	
	@Override
	public void onContactRefused(String username) {
		//好友请求被拒绝
	}
	
	@Override
	public void onContactInvited(String username, String reason) {
		//收到好友邀请
	}
	
	@Override
	public void onContactDeleted(List<String> usernameList) {
		//被删除时回调此方法
	}
	
	
	@Override
	public void onContactAdded(List<String> usernameList) {
		//增加了联系人时回调此方法
	}
});
```
## 获取黑名单
```java
//获取黑名单用户的usernames
EMContactManager.getInstance().getBlackListUsernames();
```
## 加入黑名单
```java
//第二个参数如果为true，则把用户加入到黑名单后双方发消息时对方都收不到；false,则
//我能给黑名单的中用户发消息，但是对方发给我时我是收不到的
EMContactManager.getInstance().addUserToBlackList(username,true);
```
## 从黑名单中移除
```java
EMContactManager.getInstance().deleteUserFromBlackList(username);
```
## 网络异常监听
```java
//注册一个监听连接状态的listener
EMChatManager.getInstance().addConnectionListener(new MyConnectionListener());

//实现ConnectionListener接口
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