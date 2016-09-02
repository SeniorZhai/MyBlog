title: Android、iOS大不同(七)
date: 2014-09-07 19:20:13
categories: iOS
tags: [Android,iOS,Java,Objective-C,大不同]
---
项目的文件结构上Android和iOS有很大的区别，注意点是iOS项目的文件是逻辑文件(并不一定和实际存放位置相同)
<!--more-->
## iOS项目文件结构
	├ *** 项目文件夹
	    ├ *.m *.h 项目的代码源文件
	    └ Supporting Files 资源文件夹，包含非源代码和资源文件
	        ├ ***.plist 属性列表文件，保存iOS应用的各种相关信息
	        ├ Info.strings 保存各种字符串的文本文件，主要用于程序国际化提供支持
	        ├ main.m 包含main函数，应用程序的入口
	        └ ***-Prefix.pch 来自外部框架的头文件，是预编译的头文件。
	├ ***Tests 包含单元测试的相关类和资源
	├ Frameworks 项目依赖的框架或库，也可以包含图像和声音等资源
	└ Products 包含项目所生成的应用程序
## Android项目文件结构
	├ ***
		├ src 存放代码源文件
		├ lib 库文件
	    ├ res 资源文件
	    	├ values 存放字符串、颜色、尺寸等资源文件
	    	├ layout 布局资源文件
	    	└ drawable 图片资源文件，可以是真实的图片，也可以是xml格式的配置图片
	    └ AndroidManifest.xml 系统清单文件，控制Android应用的各项配置