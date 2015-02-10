title: Android事件
date: 2015-02-04 16:47:29
categories: Android 
tags: [EventBus]
---
[EventBus](https://github.com/greenrobot/EventBus)是一个非常流行的开源库，用于Android事件发布/订阅，通过解耦发布者和订阅者简化Android事件传递。事件传递可以用于Android四大组件也可以作用于异步线程和主线程的通讯。
相比`Handler`、`BroadCastReceiver`、`Interface回调`，EventBus的优点代码更简洁，使用简单将事件发布和订阅充分解耦。
<!--more-->
##概念
- 事件(Event)：分为一般事件和Sticky事件，相对于一般事件，Sticky事件在事件发布后，再有订阅者开始订阅该类事件，依然能收到该类型事件最后一次Sticky事件。
- 订阅者(Subscriber)：订阅某种事件类型的对象，当有发布者发布这类事件后，EvenrBus会执行订阅者的`onEvent`函数，订阅者通过`register`接口订阅某个事件，`unregister`退订。订阅者存在优先级，优先级高的订阅者可以取消事件的分发，默认所有订阅者的优先级都是0.
- 发布者(Publisher)：发布某事件的对象，通过post接口发布事件

##使用
```java
public class Activity1 extends Activity {
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity1);
		EventBus.getDefault().register(this); //注册对象
		new Thread(new Runnable() {
            int i= 1;
            @Override
            public void run() {
                while(true){
                    try {
                        Thread.sleep(2000);
                        EventBus.getDefault().post(new EventA("正在运行第"+i+"次");
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }).start();
	}	
	@Verride
	protected void onDestroy() {
		// 注销
		EventBus.getDefault().unregister(this);
		super.onDestroy();
	}
	public void onEventMainThread(EventA eventA){
        TextView tv = (TextView) findViewById(R.id.tv);
        tv.setText(eventA.text);
    }
}
public class EventA {
    String text = "asd";
    public EventA(String text){
        this.text = text;
    }
}
```
###ThreadMode
- onEventMainThread表示这个方法会在UI线程中运行
- onEventPostThread表示这个方法会在当前发布事件的线程执行
- onEventBackgroundThread如果在非UI线程发布的事件，直接执行和发布的线程在同一个线程，如果UI线程发布则加入到一个后台线程队列中
- onEventAsync代表方法直接在独立的线程中去执行