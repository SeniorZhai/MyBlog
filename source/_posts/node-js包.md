title: node.js包
date: 2015-01-16 20:39:34
categories: node.js
tags: [package,包]
---
复杂的模块往往由多个子模块组成，为了便于管理和使用，多个子模块组成的大模块称为`包`，并将所有子模块放在同一个目录里
<!--more-->
文件结构一般如下
```
- cat/
	head.js
	body.js
	main.js
```
`main.js`作为入口模块
```js
var head = require('./head')
var body = require('./body')

export.create = function(name) {
	return {
		name:name,
		head:head.create(),
		body:body.create()
	}
}
```
当模块名为`index.js`加载模块时可以使用模块所在目录的路径替代模块文件路径
以下两句话是等价的
```js
var cat = require('/home/user/lib/cat');
var cat = require('/home/user/lib/cat/index');
```
`package.json`可以自定义入口模块的文件名和存放位置

一般的工程目录就会如下
```
-bin/	# 命令行相关代码
	node-echo
+ doc/	# 文档
- lib/	# 存放API相关代码
	echo.js
- node_modules/ 	# 存放第三方包
	+ argv/
package.json 	# 元数据文件
README.md 	# 说明文件
```
