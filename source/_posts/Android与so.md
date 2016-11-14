title: Android与so
date: 2016-12-07 17:25:41
categories: Android
tags: [ndk,so]
---

<!--more-->

##ABI

ABI（Application Binary Interface）应用二进制借口，定义了CPU能够执行的二进制文件的格式规范。
Android支持的CPU架构众多，ABI包括`armeabi`、`armeabi-v7a`、`arm64-v8a`、`x86`、`x86_64`、`mips`、`mips64`
> https://developer.android.com/ndk/guides/abis.html#sa

so (shared object)是机器可以直接运行的二进制代码，买个App的so被打包在apk文件的`lib/`目录下
```
lib
|
├── armeabi
│   └── libmath.so
├── armeabi-v7a
│   └── libmath.so
├── mips
│   └── libmath.so
└── x86
    └── libmath.so
```
使用下面命令可以查看apk支持的abi
```shell
appt dump bading app.apk | grep abi
```

##制定ABI生产so

在`Application.mk`文件中指定`APP_ABI`参数

##Android系统的ABI支持
- primary ABI当前系统使用的ABI
- secondary ABI 当前系统支持的其他ABI
一般情况手机不止支持一个ABI

###查看ABI
- `/system/build.prop`中制定了支持的ABI类型，可以在adb shell中查看
```shell
getprop | grep abilist
```
- 使用`Build.BUPPORTED_ABIS`可以获取当前设备支持的ABI列表
```java
Build.SUPPORTED_ABIS;
```

在apk安装过程时，Package Manager会扫描整个apk寻找符合下面文件路径格式动态链接库
```
lib/<primary-abi>/lib<name>.so
```
`primary-abi`是上面表中的abi的值，`name`对应的是`Android.mk`中定义的`LOCAL_MODULE`的值
如果没有当前`primary-abi`的so，Package Manager会尝试寻找合适`secondary-abi`的so文件
```
lib/<secondary-abi>/lib<name>.so
```
有合适的so文件后，包管理会在改ABI文件下的所有so库全部拷贝到应用的data目录下`data/data/<package_name>/lib/`

在代码调用so时，如果ABI文件夹下没有提供so文件，运行时会遇到`java.lang.UnsatisfiedLinkError`

##加载so
`System.loadLibrary`之传入`Android.mk`中定义的`LOCAL_MODULE`的值
`System.load`这个方法可以动态的加载非apk内置的so，甚至动态下载的so，但是不能加载SD卡中的so
```
System.load("/data/data/<package-name/dir/libmath.so")
```
