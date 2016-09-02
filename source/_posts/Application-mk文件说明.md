title: Application.mk文件说明
date: 2015-04-13 21:44:30
categories: Android
tags: [ndk]
---
Application.mk描述Android应用程序中需要本地模块
<!--more-->
## APP_PROJECT_PATH
应用程序工程的根目录的绝对路径
这是用来复制或安装一个没有任何限制的JNI库

## APP_MODULES
可选的变量，默认定义为LOCAL_MODULE中

## APP_OPTIM
可选的变量，用来定义`release`或`debug`，在编译应用模块的时候，可以用来改变优先级。
`release`会生成高度优化的二进制代码，`debug`生成的时为优化的二进制代码，可以检测出很多BUG，可以用于调试

## APP_CFLAGS
编译器开关集合，可以用于改变一个给定应用程序需要依赖的模块的构建

## APP_ABI
默认情况下NDK的编译系统根据"armeabi"ABI生成机器代码，可以指定选择不同的ABI生成。
如`x86`，`armeabi-v7a`，`armeabi`，`all`

