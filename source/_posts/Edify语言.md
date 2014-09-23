title: Edify语言
date: 2014-08-17 14:05:00
categories: Android
tags: [Android]
---
<!--more-->
- ui_print(msg1,...,msgN) 用于在Recovery界面输出字符串，不定参数。
- run_program(prog,arg1,...,argN) 用于执行程序，其中prog参数表示执行的程序文件(要写完整路径)，arg1,...argN表示执行程序的参数，prog是必须的，arg是可选的
- delete(file1,...fileN) 用于删除一个或多个文件
- package)extract_dir(package_path,desination_path) 用于提取刷机包中的pack_path指定的目录。其中package_path参数表示刷机包中的目录，destination_path表示目标目录
- set_perm(uid,gid,mode,file1,file2,...fileN) 用于设置一个或多个文件的权限，其中uid参数表示用户ID，gid表示用户组ID。如果想让文件的用户和用户组都是root，uid和gid需要都为0。mode参数表示设置的权限，与chmod命令类似。
- mount(fs_type,partion_type,location,mount_point) 挂载分区
- unmount(mount_point) 用于解除挂载，mount_point参数表示文件系统
