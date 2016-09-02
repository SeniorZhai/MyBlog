title: Service
date: 2014-06-19 13:28:28
categories: Android
tags: [Android]
---
	- void onCreate():当Service第一次被创建后将立即回调该方法。
	- void onStartCommand(Intent intent,int flag,int startId):每次通过startService()方法启动Service时都会被回调
	- void onDestroy():当Service被关闭前会被回调
	- abstract IBinder onBind(Intent intent):该方法是Service子类必须实现的方法，如果不需要通过绑定的方式启动服务，可以返回Null
	- boolean onUnbind(Intent intent):当Service上绑定的所有客户端都被断开连接将被回调该方法

通过Service的启动方式与适用范围，可将Service分为两类服务：
	- start:启动服务，当一个Android组件（如一个Activity）调用startService()的时候，启动一个Service。Service一旦启动，就可以一直在后台运行下去，即使这个启动它的组件被摧毁。这样的服务模式，通常用于执行一个操作而不需要返回结果给调用者。
	- Bound：绑定服务，当一个Android组件（如一个Activity）调用binService()，一个绑定Service提供了一个客户端到服务端的接口，允许组件与服务之间进行交互，这样可以实现跨进程的通信。绑定服务的生命周期默认是跟随他的绑定组件的，但是一个绑定Service可以绑定多个Android组件，如果这些Android组件都被销毁，那么这个绑定服务也将被销毁。

一个Service可以包含上面两种运行方式，与它重载的方法有关，如果重写了onStartCommand()即支持启动Service，如果重写onBind()即支持绑定服务，如果重载实现了两个方法即可实现两种服务。

对于两种方式的Service，其生命周期被回调的方法也是不一样的：

![](https://github.com/zt1991616/blog/raw/master/Image/14061901.png)

## 清单文件的配置
	Service是Android的四大组件之一，所以它必须在AndroidManifest清单文件中进行配置，否则系统将找不到这个服务。Service的配置也是在<application />这个节点下，使用<service />进行配置，其中android:name属性为Service类。
	如果开发的Service需要被外部操作，还需要配置<intent-filter />节点。
	如果Service强制仅本应用操作，可以配置<service />节点的android:exported属性为false，这样即使配置了<intent-filter />，外部应用也无法操作这个服务，android:exported属性默认是true。

## Service开发步骤
1. 开发一个Service类，需要继承Service或者IntentService
2. 在AndroidManifest清单文件中注册这个Service组件
3. 在一个Android组件中启动这个Service
4. Service使用完成之后，需要停止这个服务

## 启动Service
	启动服务必须实现Service.onStartCommond()方法，启动服务使用startService(Intent intent)方法开启一个服务，仅需要传递一个Intent对象即可，在Intent对象中指定需要启动的服务，而使用startService()方法启动的服务，在服务的外部，必须使用stopService()方法停止当前的Service，该Service就会被销毁。
	对于启动服务，一旦启动将与访问它的组件无任何关联，即使访问它的组件被销毁了，这个服务也一直运行下去，直到被销毁！
```java
public class StartService extends Service{
	private final static String TAG = "StartService";
	@Override
	public IBinder onBind(Intent arg0) {
		// 仅通过startService()启动服务，所以这个方法返回null即可。
		return null;
	}
	@Override
	public void onCreate(){
		super.onCreate();
		Log.i(TAG,"Service is Create");	
	}
	@Override
    public void onCreate() {
		super.onCreate();
		Log.i(TAG, "Service is Created");        
	}

	@Override
	public int onStartCommand(Intent intent, int flags, int startId) {
    	Log.i(TAG, "Service is started");
        return super.onStartCommand(intent, flags, startId);
	}
	@Override
	public void onDestroy() {
		Log.i(TAG, "Service is Destroyed");
		super.onDestroy();
	}
}
```
```java
public class MainActivity extends Activity{
	private Button startSer,stopSer;
	private Intent intent = null;

	@Override
	protected void onCreate(Bundle saveInstanceState){
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		startSer = (Button) findViewById(R.id.startSer);
		stopSer = (Button) findViewById(R.id.stopSer);

		intent = new Intent(MainActivity,StartService.class);
		startSer.setOnClickListener(new View.OnClickListener() {
			@Override
			public void onClick(View v) {
				startService(intent);
			}
		}
		stopSer.setOnClickListener(new View.OnClickListener() {
			@Override
			public void onClick(View v) {
				stopService(intent);
			}
		}
	}
}
```

## 绑定服务
	如果Service和宿主之间需要进行方法调用或者数据交换，则应该使用Context对象的binService()和unbindSevice()方法来绑定和解除绑定服务。
	Context的binService()方法的完整方法签名为:binService(Intent service,ServiceConnection conn,int flags)
	- service:通过Intent指定要绑定的Service
	- conn:一个ServiceConnection对象，该对象用于监听访问者与Service对象的onServiceConnected()方法
	- flags:指定绑定时是否自动创建Service，0为不自动创建，BIND_AUTO_CREATE为自动创建

	从上面的bindService方法可以看出，绑定一个服务与宿主交互，依托一个ServiceConnection接口，这个接口对象必须声明在主线程中，通过实现其中的两个方法，来实现与Service的交互。
	- void onServiceConnection(ComponentName name,IBinder service)：绑定服务的时候被回调，在这个方法获取绑定Service传递过来的IBinder对象，通过这个IBinder对象，实现宿主和Service的交互。
	- void onServiceDisconnected(ComponetName name):当取消绑定时回调，但正常情况下不被调用，它的调用时机是当Service服务被意外销毁时，才被自动调用。

	在绑定Service时，必须提供一个IBinder onBind(Intent intent)方法，在绑定本地Service的情况下，onBind()方法h返回的IBinder对象会传给宿主ServiceConnection.onServiceConnected()方法发service参数，这样宿主就可用通过IBinder对象与Service进行通信。实际开发中一般会继承Binder类(IBinder的实现类)的方式来实现自己的IBinder对象。

	如果绑定服务提供的onBind返回为Null，则也可以使用bindService()启动服务，但不会绑定上Service，因此宿主的ServiceConnection.onServiceConnected()方法不会被执行。
```java
// MainActivity.java
public class MainActivity extends Activity {
	private final String TAG = "MainActivity";
	private Button bn1,bn2,bn3;
	private MyService.MyBinder binder;
	private ServiceConnection conn = new ServiceConnection() {

		@Override
		public void onServiceDisconnected(ComponentName name) {
			Log.d(TAG, "--Service Disconnected--");
		}

		@Override
		public void onServiceConnected(ComponentName name, IBinder service) {
			Toast.makeText(MainActivity.this, "绑定成功", Toast.LENGTH_SHORT).show();
			binder = (MyService.MyBinder) service;
		}
	};

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		final Intent intent = new Intent(MainActivity.this,MyService.class);
		
		bn1 = (Button) findViewById(R.id.button1);
		bn1.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				bindService(intent, conn, Service.BIND_AUTO_CREATE);
			}
		});
		bn2 = (Button) findViewById(R.id.button2);
		bn2.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				binder = null;
				unbindService(conn);
			}
		});
		bn3 = (Button) findViewById(R.id.button3);
		bn3.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				if(binder!=null){
					Toast.makeText(MainActivity.this, "Count"+binder.getCount(), Toast.LENGTH_SHORT).show();
				}else {
					Toast.makeText(MainActivity.this, "未绑定", Toast.LENGTH_SHORT).show();
				}
			}
		});
		
	}
```
```xml
<!-- activity_main.xml -->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.example.service.MainActivity" >

    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="bind" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="unbind" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="getCount" />
    
</LinearLayout>
```
```java
//MyService.java
public class MyService extends Service {
	private final static String TAG = "MyService";
	private int count;
	private boolean quit = true;

	private Thread thread;
	private MyBinder binder = new MyBinder();

	public class MyBinder extends Binder {
		public int getCount() {
			return count;
		}
	}

	@Override
	public void onCreate() {
		super.onCreate();
		thread = new Thread(new Runnable() {
			@Override
			public void run() {
				while (quit) {
					try {
						Thread.sleep(1000);
					} catch (InterruptedException e) {

					}
					count++;
				}
			}
		});
		thread.start();
	}

	@Override
	public IBinder onBind(Intent intent) {
		return binder;
	}

}
```