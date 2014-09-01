title: Anroid NDK的安装与配置
date: 2014-02-20 22:14:20
categories: Android
tags: [Android]
---
##Mac版
1. 到Google的官网下载[Andorid NDK]("http://developer.android.com/tools/sdk/ndk/index.html")
2. 解压到你要存放的位置
3. 开启终端，输入命令pico .bash_profile
![](https://github.com/zt1991616/blog/raw/master/Image/14022001.png)
4. 添加NDK的绝对路径
`export PATH=${PATH}:/Users/userName/Documents/android-ndk-r9c`
`A_NDK_ROOT=/Users/userName/Documents/android-ndk-r8d`
`export A_NDK_ROOT`
![](https://github.com/zt1991616/blog/raw/master/Image/14022002.png)
    + 注：图片中有SDK的部分，具体路径视个人情况而定。

5. 测试：关闭终端后重启，输入ndk-build,出现以下提示，证明配置成功
![](https://github.com/zt1991616/blog/raw/master/Image/14022002.png)

##Windows版
1. 到Google的官网下载[Andorid NDK]("http://developer.android.com/tools/sdk/ndk/index.html")
2. 解压到你要存放的位置
3. 计算机-属性-高级系统设置-高级-环境变量-系统环境变量-编辑`Path`变量-添加NDK的存放路径
4. 测试：在命令行中输入ndk-build,出现以下提示，证明配置成功
![](https://github.com/zt1991616/blog/raw/master/Image/14022005.png)