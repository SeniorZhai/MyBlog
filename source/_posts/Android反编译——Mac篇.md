title: Android反编译——Mac篇
date: 2015-08-06 13:12:52
categories: Android
tags: [Mac,反编译,OSX,APK]
---
目前Mac上流行的Android反编译工具有两款，分别是Jadx和AndroidDecompiler
<!--more-->
# Jadx
[Jadx](https://github.com/skylot/jadx)是一款跨平台的反编译工具，有GUI界面

- 使用方法
1. 下载Jadx
2. 运行`bin/jadx-gui`，选择APK文件
3. 可以看到Java源码，选择`File`->`Save ALL`即可保存文件

![](/img/15080601.png)

![](/img/15080602.png)

# AndroidDecompiler
[AndroidDecompiler](https://github.com/dirkvranckaert/AndroidDecompiler)是一款命令行工具

- 使用方法
1. 下载[AndroidDecompiler](https://github.com/dirkvranckaert/AndroidDecompiler)
2. 切换到它的目录下，执行`decompileAPK.sh`脚本
```
decompileAPK.sh -p xxx.apk
```
3. 在当前目录下的`output`文件夹就是反编译之后的文件

![](/img/15080603.png)

![](/img/15080604.png)