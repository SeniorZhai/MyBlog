title: 使用jadx
date: 2016-03-10 14:11:00
categories: Android
tags: [反编译]
---
<!--more-->
#使用
将[jadx](https://github.com/skylot/jadx)克隆到本地，进行编译
```shell
git clone https://github.com/skylot/jadx
cd jadx
./gradlew dist
```
编译完成后可以在`build/jadx/bin`中找到运行文件

##运行
GUI界面可以点击运行`jadx-gui`文件
命令行可以使用`./jadx`
```
jadx demo.apk
```
- 命令格式
```
jadx[-gui] [options] <input file> (.dex , .apk , .jar or .class)
options:
  -d,--output-dir         指定输出文件夹
  -j,--threads-count      执行线程数量
  -f,--fallback
  -r,--no-res             不解码资源文件
  -s,--no-src             不反编代码
     --show-bad-code      显示不正确的反编译代码
     --cfg                
     --raw-cfg
  -v,--verbose            详细输出
     --deobf              激活deofuscation
     --deobf-min
     --deobf-max
     --deobf-rewrite-cfg
  -h,--help
```

> --deobf --deobf-rewrite-cfg
