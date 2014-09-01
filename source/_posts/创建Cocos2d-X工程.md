title: 创建Cocos2d-X工程
date: 2014-02-26 14:46:37
categories: Cocos2d-X
tags: [Cocos2d-X]
---
`../cocos2d-x/tools/project-creator`目录下的create_project.py
输入`./create_project.py -project Name -package PackageName -language cpp`命令，创建一个名为`Name`，包名为`PackageName`，编程语言为c++的一个工程。
##环境配置
- .bash_profile中加上以下几个路径：
```
export COCOS2DX_ROOT=/Users/zhaitao/Documents/cocos2d-x-2.2.2
export ANDROID_SDK_ROOT=/Users/zhaitao/Documents/Android/sdk
export ANDROID_NDK_ROOT=/Users/zhaitao/Documents/Android/android-ndk-r9c
export NDK_ROOT=/Users/zhaitao/Documents/Android/android-ndk-r9c
export PATH=$PATH:$ANDROID_NDK_ROOT
export PATH=$PATH:$ANDROID_SDK_ROOT
```
- 修改build_native.sh文件
1. 在路径/Users/user/Documents/cocos2d-x-2.2.2/samples/Cpp/TestCpp/proj.android下找到build_native.sh
2. 在APPNAME="TestCpp"下添加NDK_ROOT="/Users/user/Documents/android-ndk-r9c"   
3. 命令行执行build_native.sh