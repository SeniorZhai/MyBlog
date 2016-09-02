title: Xcode插件管理大师Alcartraz
date: 2014-03-12 12:52:59
categories: iOS
tags: [iOS]
---
![](https:github.com/zt1991616/blog/raw/master/Image/14031201.jpg)
## 简介

Alcartraz是一个帮助你管理Xcode插件、模板以及颜色配置的工具，集成在Xcode的图形界面中。

## 安装与卸载

安装命令
```
mkdir -p ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins;
curl -L http://git.io/lOQWeA | tar xvz -C ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins
```
卸载命令
```
rm -rf ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins/Alcatraz.xcplugin
rm -rf ~/Library/Application\ Support/Alcatraz
```

## 使用

安装成功后重启Xcode，就可以在Xcode的顶部菜单中找到Alcatraz，如下所示：
![](https:github.com/zt1991616/blog/raw/master/Image/14031202.png)
点击“Package Manager”，即可启动插件列表页面，如下所示
![](https://github.com/zt1991616/blog/raw/master/Image/14031203.png)
之后你可以在右上角搜索插件，对于想安装的插件，点击其左边的图标，即可下载安装，如下所示，我正在安装KImageNamed插件：
![](https://github.com/zt1991616/blog/raw/master/Image/14031204.jpg)
安装完成后，再次点击插件左边的图标，可以将该插件删除。

## 插件路径

Xcode所有的插件都安装在目录`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/`下，你也可以手工切换到这个目录来删除插件。