title: Android 与 so
date: 2016-12-07 17:25:41
categories: Android
tags: [ndk,so]

---

<!--more-->

## ABI

ABI（Application Binary Interface）应用二进制借口，定义了 CPU 能够执行的二进制文件的格式规范。
Android 支持的 CPU 架构众多，ABI 包括`armeabi`、`armeabi-v7a`、`arm64-v8a`、`x86`、`x86_64`、`mips`、`mips64`

> https://developer.android.com/ndk/guides/abis.html#sa

so (shared object)是机器可以直接运行的二进制代码，买个 App 的 so 被打包在 apk 文件的`lib/`目录下

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

使用下面命令可以查看 apk 支持的 abi

```shell
appt dump bading app.apk | grep abi
```

## 制定 ABI 生产 so

在`Application.mk`文件中指定`APP_ABI`参数

## Android 系统的 ABI 支持

- primary ABI 当前系统使用的 ABI
- secondary ABI 当前系统支持的其他 ABI
  一般情况手机不止支持一个 ABI

### 查看 ABI

- `/system/build.prop`中制定了支持的 ABI 类型，可以在 adb shell 中查看

```shell
getprop | grep abilist
```

- 使用`Build.BUPPORTED_ABIS`可以获取当前设备支持的 ABI 列表

```java
Build.SUPPORTED_ABIS;
```

在 apk 安装过程时，Package Manager 会扫描整个 apk 寻找符合下面文件路径格式动态链接库

```
lib/<primary-abi>/lib<name>.so
```

`primary-abi`是上面表中的 abi 的值，`name`对应的是`Android.mk`中定义的`LOCAL_MODULE`的值
如果没有当前`primary-abi`的 so，Package Manager 会尝试寻找合适`secondary-abi`的 so 文件

```
lib/<secondary-abi>/lib<name>.so
```

有合适的 so 文件后，包管理会在改 ABI 文件下的所有 so 库全部拷贝到应用的 data 目录下`data/data/<package_name>/lib/`

在代码调用 so 时，如果 ABI 文件夹下没有提供 so 文件，运行时会遇到`java.lang.UnsatisfiedLinkError`

## 加载 so

`System.loadLibrary`之传入`Android.mk`中定义的`LOCAL_MODULE`的值
`System.load`这个方法可以动态的加载非 apk 内置的 so，甚至动态下载的 so，但是不能加载 SD 卡中的 so

```
System.load("/data/data/<package-name/dir/libmath.so")
```
