title: typeof和instanceof
date: 2015-02-25 13:51:49
categories: node.js 
tags: [js]
---
typeof和instanceof都可以用来将值分类，typeof主要用于原始值，instanceof主要用于对象。
<!--more-->
##typeof使用方法
```js
typeof <value>
```
typeof返回描述value'类型'的字符串
```js
typeof true 	// 'boolean'
typeof 'abc'	// 'string'
typeof {}		// 'object'
typeof []		// 'object'
```
详情对应下表
|操作数|结果|
|:---|:---|
|undefined|'undefined'|
|null|'object'|
|Boolean value|'boolean'|
|Number value|'number'|
|String value|'string'|
|Function|'function'|
|All other value|'object'|
##instanceof
```js
value instanceof <Constr>
var b = new Bar();
b instanceof Bar;	// true
{} instanceof Object;	// true
[] instanceof Array;	// true
[] instanceof Object;	// true
```
