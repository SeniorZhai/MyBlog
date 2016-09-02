title: 使用supervisor热启动
date: 2015-02-11 17:41:44
categories: node.js
tags: [热启动,supervisor]
---
<!--more-->
## 安装
```shell
sudo npm install -g supervisor
```
全局安装supervisor，mac上需要权限
## 应用
安装完成后，即可使用命令启动应用
```shell
supervisor app.js
```
## 在WebStorm中使用
![](/img/15021101.png)
按如上设置，Node parameters中设置`/usr/local/lib/node_modules/supervisor/lib/cli-wrapper.js --exec         /usr/local/bin/node --no-restart-on exit`