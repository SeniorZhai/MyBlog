title: 判断iOS设备种类
date: 2015-09-04 21:35:43
categories: iOS
tags: [swift]
---
<!--more-->
`UIDevice`的静态方法`currentDevice`可以获取到设备信息(UIUserInterfaceIdiom枚举类型)，包括`Phone`、`Pad`、`Unspecifid`
其中Phone为手机设备、Pad为平板设备、Unspecifid为未知设备

##各类栏的尺寸
状态栏的高度为20PX，NavigationBar为44PX，底栏为49PX