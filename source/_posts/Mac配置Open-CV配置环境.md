title: Mac配置Open CV配置环境
date: 2018-07-20 14:55:51
categories: OpenCV
tags: [OpenCV,C++]
---
<!--more-->
# 下载安装 cmake
http://cmake.org/
# 下载 opencv
https://opencv.org/releases.html
# 编译 open cv
download www.opencv.org 

open cmake
选择 opencv opencv/build
configure->generate

# 安装 open cv
```
cd opencv/build
make
sudo make install
```

# 配置 xcode
在`build setting`中添加
```
header search paths -> /usr/local/include
library search paths -> /usr/local/lib
other linker flags -> add opencv/build/lib/ # 添加所有动态库
```

为了方便，我们将图片路径以Argument是方式传入Main函数
Product->Scheme->Edit -Scheme>Run->Arguments
