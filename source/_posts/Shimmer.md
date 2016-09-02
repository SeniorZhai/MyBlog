title: Shimmer
date: 2014-03-20 12:54:24
categories: iOS
tags: [iOS]
---
[shimmer](https://github.com/facebook/Shimmer)是Facebook旗下应用Paper开源的加载效果，显示为一个闪闪发光的Label。

![](https://raw.github.com/zt1991616/blog/master/Image/14032002.gif)

## 安装
使用Cocoapods来安装FBShimmering库
1. 命令行里运行`pod search Shimmer`

![](https://raw.github.com/zt1991616/blog/master/Image/14032003.png)

2. 将上面的`pod 'Shimmer', '~> 1.0.1'`copy，在你的工程下创建`Podfile`文件，并写入
```
platform :ios, '7.0'

pod 'Shimmer', '~> 1.0.1'
```

3. 在命令行切换到你的工程目录，运行`pod install`

![](https://raw.github.com/zt1991616/blog/master/Image/14032001.png)

## 用法
创建一个`FBShimmeringView`或`FBShimmeringLayer`，添加您的内容。要开启闪烁，就设置`shimmering`属性为YES。
例子：
```Objective-C
FBShimmeringView  * shimmeringView  =  [[ FBShimmeringView  alloc ]  initWithFrame : self.view.bounds ]; 
[ self.view  addSubview : shimmeringView ];

UILabel  * loadingLabel  =  [[ UILabel  alloc ]  initWithFrame : shimmeringView.bounds ]; 
loadingLabel.textAlignment  =  NSTextAlignmentCenter ; 
loadingLabel.text  =  NSLocalizedString ( @"Shimmer" ,  nil ); 
shimmeringView.contentView  =  loadingLabel ;

/ /开始闪闪发光
shimmeringView.shimmering =  YES ;
```

## 示例链接
[示例](https://github.com/zt1991616/ShimmerDemo)：

![](https://raw.github.com/zt1991616/blog/master/Image/14032004.gif)