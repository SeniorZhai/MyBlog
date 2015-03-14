title: AndroidStudio调用so文件
date: 2015-02-13 09:51:49
categories: Android 
tags: [AndroidStudio,so]
---
<!--more-->
1. 将*.so文件拷贝到`app\libs\armeabi`文件夹下
2. 修改`build.gradle`文件，在`buildTypes`下添加
```
sourceSets {  
        main {  
            jniLibs.srcDirs = ['libs']  
        }  
    }  
```
3. 在调用处
```java
public native String  stringFromJNI();           //jni 函数名  
public native String  getFFmpegVersionFromJNI(); //jni函数名  
static {  
    System.loadLibrary("ffmpeg");                //加载.so文件  
    System.loadLibrary("ffmpeg-jni");            //加载.so文件  
}  
```

---
另一个方法
*.so文件导入android到`app/src/main/jniLibs`文件夹下
![](/img/15021301.jpg)