title: xib文件注意点
date: 2014-02-22 14:38:34
categories: iOS
tags: [iOS]
---
1. 创建控制器. File->New File->Iphone OS->Cocoa Touch Class->UIViewController subclass;
2. 创建xib. File->New File->Iphone OS->User Interface->View XIB
3. 绑定controller和view. 用Interface Builder打开xxx.xib, 点击Files' Owner, 在Identity Inspector里面的Class Identity, 选择Step 1创建的控制器类, 接着拖拽File's Owner到View中, 选择Outlets->view.先选中file's owner(这个很重要)

![](https:github.com/zt1991616/blog/raw/master/Image/14022204.png)
![](https:github.com/zt1991616/blog/raw/master/Image/14022205.png)

##定制iOS应用图标

- 需要57X57、114X114、120X120的图片作为图标。
- 需要320X480、640X960、640X1136的素材作为启动画面。
=======