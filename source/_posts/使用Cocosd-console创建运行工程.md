title: 使用Cocosd-console创建运行工程
date: 2014-03-19 11:31:51
categories: Cocos2d-X
tags: [console]
---
## 运行环境

- Android2.3及以上版本
- iOS 5.0及以上版本
- OS X 10.7及以上版本
- Windows 7及以上版本
- Ubuntu 12.04及以上版本
- Cocos2d-X v3.0rc

## 软件需求

- Xcode 4.6
- gcc 4.7
- Visual Studio 2012
- Python 2.7.5


## 创建新工程
切换到cocos的根目录
```
cocos new MyGame -p com.zoe.MyGame -l cpp -d ./MyProject
```
- `MyGame`:指定工程名
- `- p com.zoe.MyGame`:指定Android的包名
- `-l cpp`:指定项目的语言，可用`cpp`或者`lua`
- `-d ./MyProject`:指定存放目录
![](https://github.com/zt1991616/blog/raw/master/Image/13031901.png)
成的项目的文件夹结构如下：
![](https://github.com/zt1991616/blog/raw/master/Image/13031902.png)
## 编译运行新工程

```
cocos run -s ./MyProject/MyGame -p ios
```
- `-s`:指定工程目录，必须是绝对目录且有效的
- `-p`:选着运行平台，可选项有`ios`,`android`,`win32`,`mac`,`linux`

 ![](https://github.com/zt1991616/blog/raw/master/Image/13031903.png)