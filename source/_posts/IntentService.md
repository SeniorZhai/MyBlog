title: IntentService
date: 2014-09-17 15:22:47
categories: Android
tags: [IntentService]
---
IntentService是简单后台任务操作的理想选择。
<!--more-->
- 不可以直接和UI做交互，为了显示他执行的结果，需要发送给Activity
- 工作任务队列是顺序执行的，如果一个任务正在IntentService中执行，此时发送的请求，会等到上一个任务执行完毕
- 正在执行的任务无法打断
## 创建IntentService
定义一个类继承`IntentService`
```java
public class RSSPullService extends IntentService {
	@Ovrride
	protected void onHandleIntent(Intent workIntent) {
		String dataString = workIntent.getDataString();
		// ...
	}
}
```
> 注：在IntentService中要避免重载`onStartCommand()`等回调方法
## 在Manifest文件中定义IntentService
```xml
<application
	android:icon="@drawable/icon"
	android:label="@string/app_name">
	...
	<service
		android:name=".RSSPullService"
		android:exported="false"/>
	...
<application/>
```
## 发送任务到IntentService
可以在Activity或者Fragment的任何时间点执行Intent，触发IntentService执行任务
```java
// 创建一个显式的Intent来启动IntentService
mServiceIntent = new Intent(getActivity(),RSSPullService.class);
mServiceIetent.setData(Uri.parse(dataUrl));
// 执行
getActivity().startService(mServiceIntent);
```
一旦执行了`startSevice()`，IntentService在自己本身`onHandleIntent()`方法里面开始执行任务
## 报告后台执行状态
推荐使用`LocalBroadcastManager`(只在自己的App中执行传递的Brocadcast)
```java
public final class Constans {
	public static final String BROADCAST_ACTION = "com.example.android.threadsample.BROADCAST";
	public static final String EXTENDED_DATA_STATUS = "com.example.android.threadsamlpe.STATUS";
}
public class RSSPullService extends IntentService {
	...
	Intent localIntent = new Intent(Constans.BROADCAST_ACTION).putExtra(Constans.EXTENDED_DATA_STATUS,status);
	LocalBroadcastManager.getInstance(this).sendBroadcast(localIntent);
}
```
## 接收数据
```java
private class ResponseReceiver extends BroadcastReceiver {
	private DownloadStateReceiver() {
	}

	@Ovrride
	public void onReceive(Context context,Intent intent) {
		//
	}
}
```
```java
public class DisplayActivity extends FragmentActivity {
	public void onCreate(Bundle stateBundle) {
		super.onCreate(stateBundle);
		IntentFilter mStatesIntentFilter = new IntentFilter(Constants.BROADCAST_ACTION);
		mStatusIntentFilter.addDataScheme("http");
	}
}
```