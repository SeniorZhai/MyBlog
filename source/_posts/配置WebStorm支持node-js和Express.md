title: 配置WebStorm支持node.js和Express
date: 2015-01-19 10:09:09
categories: node.js
tags: [WebStorm,node.js,Express]
---
<!--more-->
- 打开设置选择`Node.js and NPM`，选择`node`命令行路径(已安装会自动检测)，再选择下载或导入node的源码
![](/img/15011901.png)
- 之后可以在下面的Packages中管理模块，可以选择安装各个模块，包括`express`
- 之后在`Libraies`中选择Node.js的支撑
![](/img/15011902.png)

- 运行Express项目
由于Express 4.0采用了scripts的启动方式，所以`node app.js`不能启动应用
采用`npm start`可以启动应用
在`WebStorm`上可以这么设置
![](/img/15011903.png)