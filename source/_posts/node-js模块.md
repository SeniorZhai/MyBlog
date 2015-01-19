title: node.js模块
date: 2015-01-16 20:32:34
categories: node.js
tags: [module,模块]
---
模块的基本单位就是单个JS文件
<!--more-->
- require
`require`函数用于在当前模块中加载和使用别的模块，传入一个模块名，返回模块导出对象
```js
var foo1 = require('./foo')
```
- exports
`exports`对象是当前模块的导出对象，用于导出模块共有方法和属性
```js
exports.hello = function() {
	console.log('Hello World!');
}
```
- module
通过`module`对象可以访问到当前模块的一些相关信息，但更多时候用于替换当前模块的导出对象
```js
module.exports = function() {
	console.log('Hello World!');
};
```
	模块仅在第一次被使用的时候执行初始化
