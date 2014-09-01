title: Eclipse一些设置
date: 2014-08-26 13:53:22
categories: Android
tags: [Android]
---
##显示行号
General -> Editors -> Text Editors -> Show line numbers
##使用快捷键新建Android工程
General -> Keys -> Command 在其中找到New(Android Application Project) 设置为 Ctrl+Alt+A 
##默认编码设置为UTF-8
General -> Workspace -> Text file encoding
##XML代码保存后自动格式化
Android --> Editors --> Always remove empty lines between elements：不要勾选，以确保个元素之间都有一个空行；Format on Save：勾选。
##Java代码保存时自动格式化：
Java --> Editor --> Save Actions --> Perform the selected actions on save
--> 勾选 Format source code（如果是一个人写代码，可以勾选Format all lines；如果需要通过SVN/Git等与他人合作，一定不能勾选Format all lines而是勾选Format edited lines）
##Java代码中键入分号“;”、花括号“}”时自动调整位置：
Java --> Editor --> Typing --> Automatically insert at correct position 勾选Semicolons(分号)，Braces(花括号)

##安装color theme
Help→Install New Software -> Add 添加 http://eclipse-color-theme.github.com/update 然后下一步下一步就好
安装完成 Window→Preferences→General→Appereance→Color Theme 选择自己喜欢的theme换上即可