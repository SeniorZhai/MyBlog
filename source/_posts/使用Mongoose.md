title: 使用Mongoose
date: 2015-03-05 11:11:52
categories: node.js
tags: [Mongoose,MongoDB]
---
Mongoose是MongoDB的一个对象模型工具，是基于node-mongodb-native开发的MongoDB nodejs驱动，可以在异步的环境下执行。同时它也是针对MongoDB操作的一个对象模型库，封装了MongoDB对文档的的一些增删改查等常用方法，让NodeJS操作Mongodb数据库变得更加灵活简单。
<!--more-->
## 安装及引用
1. 安装
	```shell
	npm install mongoose
	```
2. 引用mongoose
	```js
	var mongoose = require("mongoose");
	```
3. 使用`mongoose`链接数据库
	```js
	var db = mongoose("mongodb://user:pass@localhost:port/database");
	```
4. 示例
	```js
	var mongoose = require("mongoose");
	var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
	db.connection.on("error", function (error) {
    	console.log("数据库连接失败：" + error);
	});
	db.connection.on("open", function () {
    	console.log("------数据库连接成功！------");
	});
	```

## MongoDB基础
MongoDB——是一个对象数据库，没有表和行等概念，没有固定的模式和结构，所有的数据以Document的形式存储，多个Document可以组成一个Collection，多个集合组成了数据库。
Document(文档)是MongoDB的核心概念，是键值对的有序集合，在JS中表示成对象，他是MongoDB的基本单元，类似于关系型数据库中的行。Collection(集合)则更像是表的概念。

### Schema
Schema —— 一种以文件形式存储的数据库模型骨架，无法直接通往数据库端，也就是说它不具备对数据库的操作能力，仅仅只是数据库模型在程序片段中的一种表现，可以说是数据属性模型(传统意义的表结构)，又或着是“集合”的模型骨架。
定义
```js
var mongoose = require("mongoose")

var TestSchema = new mongoose.Schema({
	name : {type:String},
	age : {type:Number,default:0},
	time : {type:Date,default:Date.now},
	emial : {type:String,default:''}
});
```
基本属性类型有：`字符串`、`日期型`、`数值型`、`布尔型(Boolean)`、`null`、`数组`、`内嵌文档`等
### Model
Model —— 由Schema构造生成的模型，除了Schema定义的数据库骨架以外，还具有数据库操作的行为，类似于管理数据库属性、行为的类。
```js
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");

// 通过Schema创建Model
var TestModel = db.model("test1", TestSchema);
```
数据库中的集合名称,当我们对其添加数据时如果test1已经存在，则会保存到其目录下，如果未存在，则会创建test1集合，然后在保存数据。
### Entity
Entity —— 由Model创建的实体，使用save方法保存数据，Model和Entity都有能影响数据库的操作，但Model比Entity更具操作性。
```js
var TestEntity = new TestModel({
	name : "Lenka",
	age : 36,
	email : "lenka@qq.com"
});
console.log(TestEntity.name); // Lenka
console.log(TestEntity.age); // 36
```
完整示例
```js
var mongoose = require("mongoose");
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
var TestSchema = new mongoose.Schema({
	name : {type:String},
	age : {type:Number,default:0},
	email : {type:String},
	time : {type:Date,default:Date.now}
});
var TestModel = db.model("test1",TestSchema);
var TestEntity = new TestModel({
	name:'helloworld',
	age:28,
	emial:'helloworld@qq.com'
});
TestEntity.save(function(err,doc){
	if(error){
		console.log("error :" + error);
	} else {
		console.log(doc);
	}
});
```
