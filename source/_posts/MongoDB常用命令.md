title: MongoDB常用命令
date: 2015-03-05 12:27:33
categories: node.js
tags: [MongoDB]
---
<!--more-->
## 启动MongoDB
指定数据库路径启动
```shell
mongod --dbpath 
```
添加`--port`也可以指定端口

## 数据库操作
---
- 切换、创建数据库	`use yourDB`
- 查询所有数据库	`show dbs`
- 删除当前使用数据库	`db.dropDatabase()`
- 从指定主机上克隆数据库	`db.cloneDatabase("127.0.0.1")`
- 从指定的机器复制数据库数据到某个数据库	`db.copyDatabase("mydb","temp","127.0.0.1")`
- 修复数据库	`db.repairDatabase()`
- 查看当前使用的数据库	`db.getName`或`db`
- 显示当前数据库状态 `db.stats()`
- 当前db版本	`db.version()`
- 查看当前数据库机器地址	`db.getMongo()`

## 集合操作
---
- 创建集合	`db.createCollection('collName',{size:20,capped:5,max:100})` 创建成功会显示`{"ok":1}`
- 得到指定名称的集合	`db.getCollection("account")`
- 得到当前数据库所有集合	`db.getCollectionNames()`
- 得到当前数据库所有集合索引的状态	`db.printCollectionStats()`

## 用户相关
---
- 添加用户	`db.addUser("name")`和`db.addUser("userName","pwd123",true)` 设置密码、是否只读
- 数据库认证	`db.auth("userName","123123")`
- 显示当前所用用户	`show users`
- 删除用户	`db.removeUser("userName")`

## 集合查询
---
- 查询所用记录	`db.userInfo.find()` 默认每页显示20条记录
- 查询去掉后的当前集合中的某列的重复数据	`db.userInfo.disinct("name")`
- 查询(等于) `db.userInfo.find({"gae":22})` 查询age==22的集合
- 查询(大于)	`db.userInfo.find({"age":{$gt:22}})` 
	小于`$lt` 大于等于`$glt` 小于等于`$lte` 
- 包含	`db.userInfo.find({name:/mongo/})`
- 开头	`db.userInfo.find({name:/^mongo/})`
- 查询指定列name、age	`db.userInfo.find({},{name:1,age:1})`
- 查询指定数据	`db.userInfo.find({age:{$gt25}},{name:1,age:1})`
- 排序	`db.userInfo.find().sort({age:1})`
	降序	`db.userInfo.find().sort({age:-1})`
- 查询前5条数据	`db.userInfo.find().limit(5)`
- 查询10条以后的数据	`db.userInfo.find().skip(10)`
- 查询5-10条数据	`db.userInfo.find().limit(10).skip(5)`
- 与查询	`db.userInfo.find({$or:[{age:22},{age:25}]})`
- 查询第一条数据	`db.userInfo.findOne()`
- 查询集合的条数	`db.userInfo.find().count()`
- 按某列进行排序	`db.userInfo.find({sex:{$exists:true}}).count()`

