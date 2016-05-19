title: Weex初探
date: 2016-05-10 17:53:42
categories: node.js
tags: [weex,Android]
---
<!--more-->
目前weex还在内测阶段，还是一个github的私有项目
##基础环境
- Node.js 4.0+
- Android SDK
- Android Studio
##如何开始
clone项目，在项目目录下执行`npm install`,`./start`
将`android/playground/app/java/com/alibaba/weex/WXMainActivity`修改`CURRENT_IP`将本地IP
之后运行项目
##简单的使用
可以看到weex的写法和原生的html很像，简单的hello weex如下：
```html
<template>
  <div>
    <text style="style-size:100px;">Hello weex!</text>
  </div>
</template>
```
