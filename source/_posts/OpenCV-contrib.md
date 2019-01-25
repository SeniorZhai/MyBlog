title: OpenCV contrib
date: 2018-09-04 17:10:19
categories: OpenCV
tags: [OpenCV,C++]
---
在OpenCV 3.0中，一些不稳定的功能被移动到独立的库(Open contrib)[https://github.com/itseez/opencv_contrib/]中
<!--more-->
需要使用的话，可以下载后，将需要的模块复制到`opencv/modules`中，然后用CMAKE编译安装
```
git clone https://github.com/opencv/opencv_contrib/ --branch 3.4 #当前版本OpenCV为3.4.3
```