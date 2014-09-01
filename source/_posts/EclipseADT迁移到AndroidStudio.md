title: EclipseADT迁移到AndroidStudio
date: 2014-07-02 13:31:41
categories: Android
tags: [Android]
---
AndroidStudio是Google I/O 2013大会上推出的Android开发环境，基于IntelliJ IDEA，和EclipseADT类似。AudioStudio使用Gradle来构建项目。Gradle是一款自动化编译部署测试工具。
- [AndroidStudio官方主页](http://developer.android.com/sdk/installing/studio.html)
- [为何 IntelliJ IDEA 比 Eclipse 更好](http://www.oschina.net/news/26929/why-intellij-is-better-than-eclipse)
- [Gradle等构建系统使用介绍](http://www.ibm.com/developerworks/cn/opensource/os-cn-gradle/)

本文主要介绍如果将EclipseADT的项目迁移到AndroidStudio。
1.为项目生成Gradle所需的文件
- 项目节本结构

![](https://github.com/zt1991616/blog/raw/master/Image/14070201.png)
- ADT版本在22以上，在项目上右击->Export，然后选择Generate Gradle build files。

![](https://github.com/zt1991616/blog/raw/master/Image/14070202.png)

![](https://github.com/zt1991616/blog/raw/master/Image/14070203.png)

- 一路next，直到finish。搞定，这时候已经生成Gradle的相关文件，此时的结构

![](https://github.com/zt1991616/blog/raw/master/Image/14070204.png)

2.在AndroidStudio中导入项目
- 打开AudioStudio，选择Import Project，选择项目的目录。OK即可导入。

![](https://github.com/zt1991616/blog/raw/master/Image/14070205.png)

- 主要就是build.gradle文件。

![](https://github.com/zt1991616/blog/raw/master/Image/14070206.png)

扩展阅读
- 参考官方说明 [Migrating from Eclipse](http://developer.android.com/sdk/installing/migrate.html)
- [AndroidStudio快捷键](http://www.android-studio.org/index.php/docs/experience/142-androidstudio-shortcut-keys)