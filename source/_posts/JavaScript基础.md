title: JavaScript基础
date: 2015-01-16 18:23:20
categories: node.js
tags: [js,javascript,基础]
---
<!--more-->
# JavaScript基础
## 变量
变量可以存放值(x=1)和表达式(y=x+1)
变量名字母、数字、少量字符($、_)组成，必须使用字母、字符(但是不建议这么做)开头，变量名对大小写敏感
### 声明
JavaScript使用`var`声明变量，声明后，默认是没有值(undefined)
- JavaScript支持重新声明，之前赋的值不会丢失
### 数据类型
- 字符串，使用单引号或双引号包括
- 数字，可以使小数可以使用指数
- 布尔，true和false
- 数组，
	var cars = new Array()
	var cars = new Array("Audi","BMW","Volvo")
	var cars = ["Adudi","BMW"]
- 对象
	var person = {name:"Bill",age:1}
	person.name
	person.age
	var person = new Object()
	person.name = "Bill"
- Undefined 没有值
- Null 通过设置null来清空数据
在JavaScript一切都是对象，如字符串
	txt = "text"
	txt.length // 4
	txt.indexOf()
	txt.replace()
	txt.search()
	