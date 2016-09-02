title: Handler
date: 2014-07-17 13:40:50
categories: Android
tags: [Android]
---
Handler直接继承Object，一个Handler允许发送和处理一个Message或者Runnable对象，并且会关联到主线程的MessageQueue中。没个Handler具有一个单独的线程，并且关联到一个消息对象的线程，就是说一个Handler有一个固定的消息队列。
Handler主要有两个作用：
- 在工作线程中发送消息
- 在UI线程中获取、处理消息

Handler把压入消息队列分为Post和sendMessage:
- Post：Post运行把一个Runnable对象压入消息队列，它的方法有：post(Runnable)、postAtTime(Runnable,long)、postDelayed(Runnable,long)。
- sendMessage:sendMessage允许把一个包含消息数据的Message对象压入到消息队列中，方法有sendEmptyMessage(int)、sendMessage(Message)、sendMessageAtTime(Message,long)、sendMessageDelayed(Message,long)。

## Post
对于Handler的Post方式来说，它会传递一个Runnable对象到消息队列中，在这个Runnable对象中，重写run()方法。一般在这个run()方法中写入需要在UI线程上的操作。

在Handler中，关于Post方式的方法有：
- boolean post(Runnable r)：把一个Runnable入队到消息队列中，UI线程从消息队列中取出这个对象后，立即执行。
- boolean postAtTime(Runnable r,long uptimeMillis)：把一个Runnable入队到消息队列中，UI线程从消息队列中取出这个对象后，在特定的时间执行。
- boolean postDelayed(Runnable r,long delayMillis)：把一个Runnable入队到消息队列中，UI线程从消息队列中取出这个对象后，延迟delayMills秒执行
- void removeCallbacks(Runnable r)：从消息队列中移除一个Runnable对象。

## Message
Message是一个final类，所有不可继承，Message封装了线程中传递的消息，对于一般的数据Message提供了`getData()`和`setData()`方法来获取与设置数据，其中操作的数据是一个Bundle对象，这个Bundle对象提供了一系列的`getXxx()`和`setXxx()`方法用于传递基本数据类型的键值对。对于复杂的数据类型，Bundle提供了两个方法，专门用来传递对象，但是这两个方法也有相应的限制，需要实现特定的接口。
- putParcelable(String key,Parcelable value)：需要传递的对象类实现Parcelable接口。
- pubSerializable(String key,Serializable value)：需要传递的对象类实现Serializable接口。
除此之外Message自带的obj属性也可以用于传值，它是一个Object类型，可以传递任何类型的对象，Message自带的如下几个属性：
- int arg1:参数一，传递不复杂数据
- int arg2：参数二，传递不复杂数据
- Object obj：传递任意的对象
- int what:定义消息码，一般用于消息的标志

对于Message对象，一般不推介直接使用构造方法创建，而是使用`Message.obtain()`这个静态方法或者`Handler.obtai()`获取，此两者都是从消息池中获取，消息的数量是有上限的，为10个。

向Handler发送消息一般分两种：一种是根据Handler对象，使用handler.sendMessage()方法来发送消息，一种是根据Handle获取Message，如`handler.obtai()`或者`Message.obtain(handler)`，该Message会有一个属性Target，调用`sendToTarget()`方法，会发送到创建时的Handler中去。
Handler中，与Message发送消息相关的方法有：

- Message obtainMessage()：获取一个Message对象。
- boolean sendMessage()：发送一个Message对象到消息队列中，并在UI线程取到消息后，立即执行。
- boolean sendMessageDelayed()：发送一个Message对象到消息队列中，在UI线程取到消息后，延迟执行。
- boolean sendEmptyMessage(int what)：发送一个空的Message对象到队列中，并在UI线程取到消息后，立即执行。
- boolean sendEmptyMessageDelayed(int what,long delayMillis)：发送一个空Message对象到消息队列中，在UI线程取到消息后，延迟执行。
- void removeMessage()：从消息队列中移除一个未响应的消息。
