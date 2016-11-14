title: vim常用命令
date: 2017-02-28 16:43:01
categories: Android
tags: [vim]
---

<!--more-->
`/<关键字>`
`:<行号>`
`dd`剪切
`yy`复制
`p`粘贴
`u`撤销
`ctrl+r`重做
`w` 保存
`wq`保存退出
`q!`退出

find + xargs + grep
```
find -name Activity.java
find -name '*.java'
find -name '*.java' | xarfs grep 'extends[ \t\n]+ActiviyManagerNative'
```


```
Android.mk
LOCAL_SHARED_LIBRARIES := libutils

#include <utils/Call.Stack.h>

CallStack stack;
stack.update();
stack.log("tag");
```

```
ninja: build stopped: subcommand failed.
build/core/ninja.mk:148: recipe for target 'ninja_wrapper' failed
make: *** [ninja_wrapper] Error 1

/******/
cd external/doclava && mm -B USE_NINJA=false
```

```
/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.12.sdk/usr/include/unistd.h:733:6: note: 'syscall' has been explicitly marked deprecated here
int      syscall(int, ...);
         ^
1 error generated.
[  1% 408/31742] host C++: libc++abi <...xternal/libcxxabi/src/cxa_demangle.cpp
ninja: build stopped: subcommand failed.
make: *** [ninja_wrapper] Error
```

去到/Applications/XCode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs，发现MacOSX10.11.sdk已经被删除，只剩下MacOSX10.12.sdk，所以首先要去下载10.11的SDK。可以去MacOSX-SDKs下载MacOSX10.11.sdk，解压拷贝到/Applications/XCode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs。为了避免下次升级的时候再被删除，可以放到~/Document/MacOSX10.11.sdk，再给它创建一个软链接：

1
$ ln -s ~/Document/MacOSX10.11.sdk /Applications/XCode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk
然后确保AOSP源码下build/core/combo/mac_version.mk文件中
mac_sdk_versions_supported := 10.9 10.10 10.11
后面不要写10.12。

```
Unsupported curl, please use a curl not based on SecureTransport
Jack server installation not found
Unsupported curl, please use a curl not based on SecureTransport
Unsupported curl, please use a curl not based on SecureTransport
[  4% 842/20846] host C++: dexdump2 <= art/dexdump/dexdump.cc
ninja: build stopped: subcommand failed.
make: *** [ninja_wrapper] Error 1

#### make failed to build some targets (01:37 (mm:ss)) ####
```

首先下载curl源码，然后执行
```
./configure --prefix=/usr/local/curl --with-ssl=/usr/local/Cellar/openssl/1.0.2d_1
make
make install
```
jack-admin kill-server
jack-admin uninstall-server
cd prebuilts/sdk/tools
jack-admin install-server jack-launcher.jar  jack-server-4.7.ALPHA.jar
