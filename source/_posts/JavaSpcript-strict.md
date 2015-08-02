title: JavaSpcript-strict
date: 2015-07-14 15:30:22
categories: node.js
tags: [设计缺陷]
---
JavaScript在设计之初不强制变量需要`var`申明
<!--more-->
不使用`var`申明的变量，会变成全局变量
为了修补JavaScript这一严重的设计缺陷，ECMA在后续的规范中退出了srict模式，在strict模式下运行的JavaScript代码，强制通过`var`申明变量，未使用`var`申明变量就使用，会导致运行错误
启用strict模式的方法就是在JS代码的第一行写上
```javascript
'use strict';
```