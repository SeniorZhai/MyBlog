title: SelectorChapek插件
date: 2015-02-13 14:08:18
categories: Android
tags: [AndroidStudio]
---
[SelectorChapek](https://github.com/inmite/android-selector-chapek)是一款帮助我们快速完成Selector的AndroidStudio插件
<!--more-->
##安装
1. 选择`Preferences→Plugins→Browse repositories`搜索`SelectorChapek`安装
2. [下载](http://plugins.jetbrains.com/plugin/7298)并在`Preferences→Plugins→Install plugin from disk`选择安装

##使用
1. 在资源文件夹上右击，如`drawable-xhdpi`
![](/img/15021302.png)
2. 选择`Generate Android Selectors`
![](/img/15021303.png)
3. selectors文件会自动生成在`drawable`文件夹下
![](/img/15021304.png)

##命名规则
为了插件的正常运行，资源文件需要正确的命名，该插件支持`.png`和`.9.png`文件的识别

|文件后缀|状态|
|:---|:---|
|_normal|(default_state)|
|_pressed|state_pressed|
|_focused|state_focused|
|_disabled|state_enabled(false)|
|_checked|state_checked|
|_selected|state_selected|
|_hovered|state_hovered|
|_checkable|state_checkable|
|_activated|state_acticated|
|_windowfocused|state_window_focused|
