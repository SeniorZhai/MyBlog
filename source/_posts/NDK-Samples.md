title: NDK Samples
date: 2014-02-22 22:15:09
categories: Android
tags: [Android]
---
- 在NDk的目录下有一个例子`\android-ndk\samples\hello-jni`
1. 将它导入到eclipse开发环境中（最好选择cpoy到自己的工作空间）
2. 在命令行进入该项目的路径，执行`ndk-build`命令，编译程序
3. 在eclipse上试运行（注：在4.x的版本要将Dependencies包去掉，貌似这个包是做低版本支持的，在高版本中会出错）
![](https://github.com/zt1991616/blog/raw/master/Image/14022406.png)
4. 运行成功

![](https://github.com/zt1991616/blog/raw/master/Image/14022407.png)

```c
//hello-jni.c
# include <string.h>
# include <jni.h>
// <jni.h>用于java的调用
jstring
// 名字即包名加函数名
Java_com_example_hellojni_HelloJni_stringFromJNI( JNIEnv* env, jobject thiz )
{
# if defined(__arm__)
  # if defined(__ARM_ARCH_7A__)
    # if defined(__ARM_NEON__)
      # define ABI "armeabi-v7a/NEON"
    # else
      # define ABI "armeabi-v7a"
    # endif
  # else
   # define ABI "armeabi"
  # endif
# elif defined(__i386__)
   # define ABI "x86"
# elif defined(__mips__)
   # define ABI "mips"
# else
   # define ABI "unknown"
# endif
    // env为java与C交互的结构，返回UTF编码的字符串
    return (*env)->NewStringUTF(env, "Hello from JNI !  Compiled with ABI " ABI ".");
}

```

```Java
public class HelloJni extends Activity
{
    @Override
    public void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        TextView  tv = new TextView(this);
        tv.setText(stringFromJNI() );
        setContentView(tv);
    }
    // 声明一个native方法，即.c的函数
    public native String  stringFromJNI();
    public native String  unimplementedStringFromJNI();
    static {
        System.loadLibrary("hello-jni");
    }
}
```

## 在adb shell中运行c程序
- 在`\android-ndk\samples\test-libstdc++`下有这样一个例子，我们进行修改后然后在设备上运行它。
1. 将jni目录下的`test-libstl.cpp`的文件进行修改

```C
# include <cerrno>
# include <cstddef>
# include <stdio.h>
int main(void)
{
  printf("Zoe\n");
    return 0;
}
```

2. 在命令行下使用`ndk-build`编译该项目
![](https://github.com/zt1991616/blog/raw/master/Image/14022408.png)
3. 将项目目录下的`\libs\armeabi\test-libstl`文件拷贝到设备的`/data/data`目录下(你需要该目录的写权限)
![](https://github.com/zt1991616/blog/raw/master/Image/14022409.png)
4. 进入设备的shell，并获取权限
![](https://github.com/zt1991616/blog/raw/master/Image/14022410.png)
5. 切换到`/data/data`目录，修改`test-libstl`文件的权限，并执行它
![](https://github.com/zt1991616/blog/raw/master/Image/14022411.png)

## 编写自己的程序
1. 在任意目录下，创建jni文件夹
2. 创建一个你的程序，如：`MyDemo.c`
```C
# include <stdio.h>
void main()
{
  int a,b,c;
  scanf("%d %d %d",&a,&b,&c);
  printf("%d+%d+%d=%d\n",a,b,c,a+b+c);
}
```
3. 创建`Android.mk`文件，注意`LOCAL_MODULE`、`LOCAL_SRC_FILES`属性
```C
LOCAL_PATH := $(call my-dir)
include $(CLEAR_VARS)
LOCAL_MODULE := MyDemo
LOCAL_SRC_FILES := MyDemo.c
include $(BUILD_EXECUTABLE)
```
4. 按上一个例子编译、拷贝、执行

![](https://github.com/zt1991616/blog/raw/master/Image/14022412.png)