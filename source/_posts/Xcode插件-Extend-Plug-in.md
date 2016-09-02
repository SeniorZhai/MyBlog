title: Xcode插件 Extend Plug-in
date: 2014-05-26 12:53:46
categories: iOS
tags: [iOS]
---
# 安装
- 在线安装
```
ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"
brew install uncrustify
```
- 下载安装
	+ 下载[XEP-*.pkg](https://raw.github.com/Centny/XEP/master/Publish/XEP-v1.2.0.pkg)和[PrjEnv.pfg](https://raw.github.com/Centny/XEP/master/Publish/PrjEnv.cfg)
	+ 安装**XEP**
	+ 把`PrjEnv.cfg`拷贝到项目的根目录(.xcodeproj文件所在的目录)
	+ 配置FULLUSERNAME和VERSION就可以生成@author和@since标签

- 修改代码格式化配置

如果想修改格式化的形式，修改 uncrustify.cfg

`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/XEP.xcplugin/Contents/Resources`

- 卸载
```
cd ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins
rm -rf XEP.xcplugin
brew uninstall uncrustify
```

- 快捷键
1. 格式化：control+shift+F(有效对象为代码、文件、项目目录)
2. 生成注释：control+shift+J,以光标所在当前行往上查找可识别对象并生成对就注释（有原来有以双斜杠开头的注释会自动加到注释的描述中去).
3. 整个文件生成注释:control+command+shift+J,同2功能，查找当前打开的文档，给所有对象生成的注释 
4. 复制一行：control+option+up/down(光标所在行)
