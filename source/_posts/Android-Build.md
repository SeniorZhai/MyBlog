title: Android Build
date: 2016-10-13 15:19:07
categories:
tags:
---
Android Build系统是Android源码的一部分，用来编译Android系统、Android SDK和相关文档。
主要有Make文件、Shell脚本以及Python脚本组成，其中最主要的是Make文件。
<!--more-->
Android包含了大量的开源项目以及许多的模块，不同产商的不同设备对Android的定制也不一样，那么如何将这些项目和模块的编译同意管理起来？如何能够在不同的操作系统上进行编译？如何在编译时能够支持面向不同的硬件设备？不同的编译类型？同事还要提供面向各个产商定制扩展？

Build系统的主要逻辑都在Make文件上，其他脚本只是起到一些辅助作用
整个Build系统中的Make文件可以分为三类：
1. Build系统核心文件，此类文件定义了整个Build系统的基础框架，这些文件位于`/build/core`目录下
2. 针对某设备的Make文件，这些文件通常位于`device`目录下
3. 针对某个模块的Make文件，这类文件的名称统一为`Android.mk`

##执行编译
```shell
source build/envsetup.sh # 引入build/envsetup.sh脚本，该脚本初始化编译环境
lunch full-eng # lunch函数的参数用来制定此次编译的目标设备以及编译类型
make -j8
```
