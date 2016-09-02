title: CocoaPods
date: 2014-11-17 09:18:42
categories: iOS
tags: [CocosPods,依赖管理]
---
CocoaPods是一个objc的依赖管理工具，本身利用ruby的依赖管理gem进行构建
<!--more-->
## 下载及安装
本地安装好Ruby环境，在Terminator中输入以下命令

	sudo gem install cocoapods

由于cocoapods.org会被`墙`，所以需要使用淘宝镜像来访问
```shell
$ gem sources --remove https://rubygems.org/
$ gem sources -a http://ruby.taobao.org/
$ gem sources -l # 查看
```
## 使用CocoaPods
示例导入[AFNetworking](https://github.com/AFNetworking/AFNetworking)
为了确定`AFNetworking`是否支持CocoaPods可以先使用CocoaPods的搜索功能验证以下
```shell
$ pod search AFNetworking
```
如果可以在终端中看到关于AFNetworking类库的一些信息，说明AFNetworking支持CocoaPods
1. 首先用XCode建立一个工程，然后关闭XCode
2. 在工程目录下创建`Podfile`文件，并写入
```shell
	platform :ios,'7.0'
	pod "AFNetworking","~> 2.0"
```
以上文本可以在开源库的github上找到，上述文本的意思是支持iOS7.0使用AFNetworking 2.0版本	
3. 在命令行中(工程目录下)，执行安装命令`pod install`
4. 完成后，可以在工程目录下看到`XXX.xcworkspace`文件，点击他就可进入已包含`AFNetworking`的工程