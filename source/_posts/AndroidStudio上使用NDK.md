title: AndroidStudio上使用NDK
date: 2015-03-19 13:29:22
categories: Android
tags: [ndk,mac,AndroidStudio]
---

<!--more-->
##准备工作
1. 下载NDK
2. 配置环境变量，在`~/.bash_profile`文件下添加
```shell
# 根据自己存放的位置指定
export NDK_ROOT=/Users/UserName/Documents/Android/android-ndk-r10
export PATH=$NDK_ROOT:$PATH
```

##新建一个项目
创建一个`MathKit`类
![](/img/15031901.png)
```java
public class MathKit {
	public static native int square(int num);

	static {
		System.loadLibrary("JniDemo");
	}
}
```
在命令行中进入目录，使用`javah`命令生成.h文件
![](/img/15031902.png)
将.h文件存放至jni文件夹下，并新建.cpp文件
![](/img/15031903.png)
根据头文件，编写cpp文件
```cpp
#include <com_zoe_ndkdemo_jni_MathKit.h>

JNIEXPORT jint JNICALL Java_com_zoe_ndkdemo_jni_MathKit_square
  (JNIEnv * env, jclass cls, jint num)
  {
    return num * num;
  }
```
在`local.properties`文件添加ndk路径
```
ndk.dir=/Users/UserName/Documents/Android/android-ndk-r10
```
在app项目中的build.gradle中的`defaultConfig`中添加
```
ndk {
	moduleName "JniDemo"
}
```
之后就可以在代码中调用
```java
 private TextView textView;
 
 @Override
 protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    textView = (TextView) findViewById(R.id.text);
    textView.setText("2*2="+ MathKit.square(2));
 }
```
**示例**：<https://github.com/SeniorZhai/NdkDemo>
