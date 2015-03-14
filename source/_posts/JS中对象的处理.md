title: JS中对象的处理
date: 2015-02-25 13:58:47
categories: node.js
tags: [对象,继承]
---
<!--more-->
##单个对象
```js
var obj = {
	name: 'Zoe',
	describe: function() {
		return 'Person named ' + this.name;
	}
};
```
使用时
```js
obj.name;	// Zoe
obj.name = 'Zhai';	// 赋值
obj.age = 22;	// 自动创建
obj.describe();	// Person named Zhai;
```
`in`操作符可以用来检测一个属性是否存在
```js
'age' in obj;	// true
'foo' in obj;	// false，不存在的属性为undefined值
```
`delete`操作符可以用来删除一个属性
```js
delete obj.age;
obj.age;	//undefined
```
##构造函数
除了作为“真正”的函数和方法，函数还在JavaScript中扮演第三种角色：如果通过new操作符调用，他们会变为构造函数，对象的工厂。构造函数是对其他语言中的类的粗略模拟。约定俗成，构造函数的第一个字母大写。例如：
```js
// 设置实例数据
function Point(x, y) {
    this.x = x;
    this.y = y;
}
// 方法
Point.prototype.dist = function () {
    return Math.sqrt(this.x*this.x + this.y*this.y);
};
```
