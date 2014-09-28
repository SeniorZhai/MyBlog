title: Sublime Text使用指南
date: 2014-09-28 14:33:22
categories: Prose
tags: [Sublime Text]
---
<!--more-->
##安装Package Control
Sublime Text支持大量插件，Package Control能够帮助我们管理这些插件，它可以方便的浏览、安装和卸载Sublime Text中的插件
- 使用Ctrl+`打开Sublime Text控制台
- 输入下面命令道控制台
```shell
import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
- 等待Package Control安装完成，使用`Ctrl + Shift + P`打开命令板
##基本操作
- `Ctrl + Enter`在当前行下面增加一行并跳至该行
- `Ctrl + Shift + Enter`在当前行上增加一行并跳至该行
- `Ctrl + ←/→`逐词移动
- `Ctrl + Shift + ←/→`逐词选择
- `Ctrl + ↑/↓`移动显示区域
- `Ctrl + Shift + ↑/↓`移动当前行
##选择
- `Ctrl + D`选择当前光标所在的词并高亮该词出现的位置，再次按下选择该词下次出现的位置
- `Ctrl + K`在多重选词的过程中进行跳过，使用`Ctrl + U`进行回退，使用`Esc`退出多重编辑
![](/img/14092801.gif)
- `Ctrl + Shift + L`将当前选中区域打散，然后进行同时编辑
![](/img/14092802.gif)
- `Ctrl + J`将当前选中的区域合并成一行
![](/img/14092803.gif)
##查找&替换
Sublime Text的查找功能可以分为**快速查找**、**标准查找**、**多文件查找**
###快速查找
使用`Shift + ←/→`或者`Ctrl + D`选中关键字，然后使用`F3`跳到其下一个出现的位置，`Shift + F3`跳到其上一个出现的位置，`Alt + F3`可以选中其出现的所有位置
![](/img/14092804.gif)
###标准查找&替换
使用`Ctrl + F`调出搜索框进行搜索
![](/img/14092805.jpg)
使用`Ctrl + H`调出替换框，`Ctrl +_Shift + H`替换当前关键字，`Ctrl + Alt + H`替换所有匹配关键字
![](/img/14092806.jpg)
####关键字查找&替换
- `Alt + C`切换大小写敏感模式
- `Alt + W`切换整字匹配模式
###多文件搜索
`Ctrl + Shift + F`开启多文件搜索&替换
![](/img/14092807.jpg)
##跳转
- `Ctrl + P`会列出当前打开的文件（或者是当前文件夹的文件），输入文件名然后Enter跳转至该文件。
![](/img/14092808.gif)
- `Ctrl + R`会列出当前文件中的符号（例如类名和函数名，但无法深入到变量名），输入符号名称Enter即可以跳转到该处。此外，还可以使用F12快速跳转到当前光标所在符号的定义处（Jump to Definition）。
![](/img/14092809.gif)
- `Ctrl + G`然后输入行号以跳转到指定行。
![](/img/14092810.gif)
###组合跳跃
在`Ctrl + P`匹配文件后，可以进行后续输入以跳转到更精确的位置
- `@`符号跳转：输入`@demo`跳转到`demo`符号所在的位置
- `#`关键字跳转：输入`#keyword`跳转到`keyword`所在的位置
- `:`行号跳转：输入:12跳转到文件的第12行。
![](/img/14092811.gif)
##文件夹
用`Ctrl + K`, `Ctrl + B`显示或隐藏侧栏，使用`Ctrl + P`快速跳转到文件夹里的文件。
![](/img/14092814.jpg)
- 使用`Ctrl + Shift + N`创建一个新窗口
- `Ctrl + N`在当前窗口创建一个新标签，`Ctrl + W`关闭当前标签，`Ctrl + Shift + T`恢复刚刚关闭的标签。
- 分屏：`Alt + Shift + 2`进行左右分屏，`Alt + Shift + 8`进行上下分屏，`Alt + Shift + 5`进行上下左右分屏（即分为四屏）。
	+ 分屏下，使用`Ctrl + 数字`键跳转到指定屏，使用`Ctrl + Shift + 数字键`将当前屏移动到指定屏
- `F11` 全屏
- `Shift + F11` 切换无干扰全屏
##格式化
手动格式化使用`Ctrl + [`向左缩进，`Ctrl + ]`向右缩进，`Ctrl + Shift + V`以当前缩进粘贴代码
此外也可以使用插件实现自动缩进和智能对齐
- [HTMLBeautify](https://sublime.wbond.net/packages/HTMLBeautify)：格式化HTML。
- [AutoPEP8](https://sublime.wbond.net/packages/AutoPEP8)：格式化Python代码。
- [Alignment](https://sublime.wbond.net/packages/Alignment)：进行智能对齐。
##中文输入法Bug
Sublime Text一直存在一个Bug：中文输入框不跟随。
![](/img/14092812.jpg)
解决方法是安装`IMESupport`插件，之后重启Sublime Text问题就解决了。
![](/img/14092813.jpg)

