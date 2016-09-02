title: "npm镜像"
date: 2015-03-05 09:25:33
categories: node.js
tags: [node,npm,镜像]
---
npm全称`Node Package Manager`，是node.js的模块依赖管理工具，由于npm的源在国外，所以国内使用起来————`你懂的`。
<!--more-->
下面是一部分国内优秀的npm镜像资源
## 国内NPM镜像
---
### 淘宝npm镜像
- 搜索地址：`http://npm.taobao.org`
- registry：`http://registry.npm.taobao.org`

### cnpmjs镜像
- 搜索地址：`http://cnpmjs.org`
- registry：`http://r.cnpmjs.org`

## 使用
---
1. 临时使用
	```shell
	npm --registry https://registry.npm.taobao.org install express
	```
2. 持久是欧诺个
	```shell
	npm config set registry http://registry.npm.taobao.org
	npm config get registry ##  验证查看
	npm info express ##  验证
	```
3. 通过cnpm使用(强烈建议使用)
	```shell
	npm install -g cnpm --registry=https://registry.npm.taobao.org
	##  使用
	cnpm install express
	```