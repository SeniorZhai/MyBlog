title: Flutter desktop 运行
date: 2019-05-16 11:31:41
categories: Flutter
tags: [iOS,Android,desktop]

---

在 Google io 2019 上，官方正式宣布[Flutter](https://developers.googleblog.com/2019/05/Flutter-io19.html?m=1)支持全平台

<!--more-->

Flutter 除了 Android、iOS，也将支持 web、桌面、嵌入式

这里我们先尝试下运行下 [Flutter desktop](https://github.com/google/flutter-desktop-embedding)

## 准备

确保本地的 Flutter 是可用的，如果尚未安装，可以按照官方的[文档](https://flutter.dev/)进行安装，之后运行`flutter doctor`检测开发环境是否可用

```shell
flutter doctor
```

因为 Flutter desktop 是实验性特性，在稳定版本的 Flutter 暂时是没有的，所有需要切换 Flutter 的版本

```shell
> flutter channel
Flutter channels:
  beta
  dev
  master
* stable
```

运行`flutter channel`命令可以看到当前所在的版本

- master 最新最新的版本，有新特性新功能，也伴随着新 bug
- dev 经过全面测试的版本，相比 master 会更稳定
- beta 每个月最稳定的 dev 版本会升级成 beta
- stable 稳定版，生产环境建议使用该版本

切换到 flutter master

```shell
> flutter channel master # 切换到master
> flutter channel # 检测切换是否成功
> flutter upgrade # 升级
> flutter doctor # 检测flutter环境
```

## Flutter desktop

下载[Flutter desktop](https://github.com/google/flutter-desktop-embedding)项目

```shell
> git clone https://github.com/google/flutter-desktop-embedding
```

在项目目录下的`example`文件夹下运行

```shell
> flutter run
```

运行成功可以看到
![](/img/19051601.png)
**大功告成，Over**
