title: Android模拟事件
date: 2015-01-05 13:50:31
categories: Android
tags: [模拟,事件,测试]
---
<!--more-->
##使用Shell命令
Android中自带一个input工具，使用方法如下
1. 模拟按键操作
```shell
	adb shell # 进入系统
	input keyevent KEYCODE_BACK # 模拟返回键
	input keyevent keyevent 3 # 模拟返回键
```
常见的按键可以在<http://developer.android.com/reference/android/view/KeyEvent.html>查看
2. 对获得焦点的输入框，输入文本
```shell
	input text hello # 输入hello文本
```
输入的文本不能带空格，也不能是中文
3. 模拟点击屏幕事件
```shell
	input tap 100 200 # 在屏幕坐标(100,200)处点击
```
	
	注：坐标是从左上角开始计算的

还可以模拟长按、滑动等

- 命令 input [<source>] <command> [<arg>...]
	+ source 指定输入设备
		- trackball 轨迹球
		- joystick 操控杆
		- touchnavigation
		- mouse
		- keyboard
		- gamepad
		- touchpad
		- dpad
		- stylus
		- touchscreen
	+ command 指定动作
		- text <string> (默认设备touchscreen)
		- keyevent [--longpress] <key code number or name> ... (默认设备keyboard)
		- tap <x> <y> (默认设备touchscreen)
		- swipe <x1> <y1> <x2> <y2> `[duration(ms)]` (默认设备touchscreen)
		- press <dx> <dy> (默认设备trackball)
		- roll <dx> <dy> (默认设备trackball)
		- tmode <tmode>

##使用Instrumentation
[Instrumentation是Android](http://developer.android.com/tools/testing/index.html)用来测试的工具，可以监测系统与应用程序之间的交互，可以使用他发送按键或者触屏事件

- 发送按键
```java
Instrumentation mInst = new Instrumentation();
mInst.sendKeyDownUpSync(KeyEvent.KEYCODE_CAMERA); // 同步发送一个按下和弹起事件
```

- 触屏事件
```java
Instrumentation mInst = new Instrumentation();  
mInst.sendPointerSync(MotionEvent.obtain(SystemClock.uptimeMillis(),  
    SystemClock.uptimeMillis(), MotionEvent.ACTION_DOWN, x, y, 0);
mInst.sendPointerSync(MotionEvent.obtain(SystemClock.uptimeMillis(),  
    SystemClock.uptimeMillis(), MotionEvent.ACTION_UP, x, y, 0);
```

- 发送文本
```java
sendPointerSync("text");
```

	注：以上代码都需要权限的支持，需要在AndroidManifast.xml中添加<uses-permission android:name="android.permission.INJECT_EVENTS"/> ，但还有一些复杂的问题，可以参考[这里](http://stackoverflow.com/questions/5383401/android-inject-events-permission/7328055#7328055)


##使用内部API
在Android系统中，有些内部的API提供注入事件的方法。因为是内部API，在不同版本上可能变化比较大。使用如果想在普通App中使用，可能需要通过反射机制来调用。

在Android API 16之前，WindownManager有相应的方法提供注入事件的方法，如下：
```java
IBinder wmbinder = ServiceManager.getService("window");  
IWindowManager wm = IWindowManager.Stub.asInterface(wmbinder); //pointer  
wm.injectPointerEvent(myMotionEvent, false); //key  
wm.injectKeyEvent(new KeyEvent(KeyEvent.ACTION_DOWN, KeyEvent.KEYCODE_A), false);  
wm.injectKeyEvent(new KeyEvent(KeyEvent.ACTION_UP, KeyEvent.KEYCODE_A), false); //trackball  
wm.injectTrackballEvent(myMotionEvent, false);  
```
在API 15之后，引入了InputManager，把上面的哪些injectXXXEvent()方法从WindowManager中移除了。使用方法类似：
```java
IBinder imBinder = ServiceManager.getService("input");  
IInputManager im = IInputManager.Stub.asInterface(imBinder);

//inject key event
final KeyEvent keyEvent = new KeyEvent(downTime, eventTime, action,  
    code, repeatCount, metaState, deviceId, scancode, 
    flags | KeyEvent.FLAG_FROM_SYSTEM |KeyEvent.FLAG_KEEP_TOUCH_MODE | KeyEvent.FLAG_SOFT_KEYBOARD, 
    source);
event.setSource(InputDevice.SOURCE_ANY)  
im.injectInputEvent(keyEvent, InputManager.INJECT_INPUT_EVENT_MODE_WAIT_FOR_FINISH);

//inject pointer event
motionEvent.setSource(InputDevice.SOURCE_TOUCHSCREEN);  
im.injectInputEvent(motionEvent, InputManager.INJECT_INPUT_EVENT_MODE_WAIT_FOR_FINISH);  
```
从API 16开始，InputManager就成了一个公开的类了，可以通过如下方法获得InputManager实例：
```java
InputManager im = (InputManager) getSystemService(Context.INPUT_SERVICE);  
```
	
	注意，使用injectEvent()需要申明android:name="android.permission.INJECT_EVENTS"权限。

4. 使用Nativ/JNI调用
察看Android设备的/dev/input/目录下的设备：
```shell
shell@user:/dev/input $ ll  
crw-rw---- root     input     13,  64 2013-08-11 18:00 event0  
crw-rw---- root     input     13,  65 2013-08-11 18:00 event1  
crw-rw---- root     input     13,  66 2013-08-11 18:00 event2  
crw-rw---- root     input     13,  67 2013-08-11 18:00 event3  
crw-rw---- root     input     13,  68 2013-08-11 18:00 event4  
crw-rw---- root     input     13,  69 2013-08-11 18:00 event5  
```
可以看到有一些输入设备节点，同时也提供了一些shell工具来操作这些设备，例如上面第1节中提到的input命令，另外还有getevent和sendevent工具分别来监听和发送事件。这些方法，都可以通过JNI的方式调用。这里需要注意的时间eventX设备都是input的用户组，要直接使用，需要root设备。

特别的是，这里有一个开源项目android-event-injector，使用JNI方法注入事件。当然设备需要root。