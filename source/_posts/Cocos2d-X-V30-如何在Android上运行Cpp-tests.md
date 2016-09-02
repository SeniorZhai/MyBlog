title: Cocos2d-X V3.0 如何在Android上运行Cpp-tests
date: 2014-03-18 11:30:15
categories: Cocos2d-X
tags: [Cocos2d-X,Android,Demo]
---
## 环境要求
Mac OSX
### Cocos2d-x
首先，要先下载好[Cocos2d-x](http://cocos2d-x.org/download)，并解压到你要放的位置。
打开**Cocos2D-X**文件夹你应该看到如下的情况:
![](https://github.com/zt1991616/blog/master/raw/Image/14031805.png)

### JDK，SKD和NDK

下载[Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html),安装后，通过`java -version`进行验证

下载[SDK](https://developer.android.com/sdk/index.html?hl=sk),[NDK](https://developer.android.com/tools/sdk/ndk/index.html)解压到你存放的位置

### Python和ant

Python作为Mac的一等公民，Macbook上是默认自带了的。
可以通过`python --version`验证：
```
->	python --version
Python 2.7.5
```
如果没有安装可以通过`brew install python`进行安装
如果brew也没有，可以到[brew](http://brew.sh/)官网按照指定命令安装

之后我们还需要安装ant
```
brew install ant
```
通过`ant -version`进行验证
```
ant -version
Apache Ant(TM) version 1.9.3 compiled on December 23 2013
```

### 初始化环境
切换到`./cocos2d-x`的目录下（根据实际存放的位置）执行`.\setup.py`或者`python setup.py`运行设置脚本
根据提示分别填写NSK、SDK、ant的路径（根据实际存放位置，ant可以通过`which ant`命令查看位置）
![](https://github.com/zt1991616/blog/raw/master/Image/14031806.png)
![](https://github.com/zt1991616/blog/raw/master/Image/14031807.png)
![](https://github.com/zt1991616/blog/raw/master/Image/14031808.png)

最后，执行下面的命令完成设置
```
source ~/.bash_profile
```

## 编译
切换到cocos2d-x的build目录下
执行下面命令
```
python android-build.py -p 10 cpp-test
```
