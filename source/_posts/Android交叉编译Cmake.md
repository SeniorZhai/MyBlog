title: Android交叉编译Cmake
date: 2018-07-21 11:51:17
categories:
tags:
---
CMake是一个开源的跨平台自动化构建系统
<!--more-->
## 基本语法
- 使用`#`作为注释
- 使用`${}`取值，在IF控制语句中直接使用变量名
- 指令名(参数1 参数2……)，其中参数之间使用空格或分好隔开
- 指令与大小写无关，但是参数和变量大小写相关
> 指令大小写无关，官方建议使用大写，不过Android的Cmake指令是小写，便于阅读

## CMake常用指令
- `project(<projectname>[CXX][C][Java])`
定义工程名并制定支持的语言，默认支持所有语言
此条指令隐式定义了两个变量：`<project>_BINARY_DIR`和`<project>_SOURCE_DIR`

- `set(VAR [VALUE])`
显式定义变量，多个变量使用空格或分号隔开

- `message([SEND_ERROR|STATUS|FATAL_ERROR]"message to display")`
这个指令用于向终端输出用户定义的信息，包含了三种类型：
SEND_ERROR，产生错误，生成过程被跳过
STATUS，输出前缀为—-的信息；（由上面例子也可以看到会在终端输出相关信息）
FATAL_ERROR，立即终止所有 CMake 过程

- `add_executable(executable_file_name [source])`
将一组源文件 source 生成一个可执行文件。 source 可以是多个源文件，也可以是对应定义的变量

- `cmake_minimun_required(VERSION 3.4.1)`
用来指定 CMake 最低版本为3.4.1

CMakeList.txt
定义了需要编译的文件以及其他库的关系
