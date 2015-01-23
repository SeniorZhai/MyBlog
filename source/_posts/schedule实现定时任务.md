title: schedule实现定时任务
date: 2015-01-22 16:56:37
categories: node.js
tags: [定时任务]
---
[node-schedule](https://github.com/mattpat/node-schedule)可是实现强大的定时任务功能
<!--more-->
安装无需赘述`npm install node-schedule`
##使用步骤
1. 设定时间
2. 设定任务
###确定日期的任务
```js
var schedule = require('node-schedule');
var date = new Date(2015,1,22,16,30,0);

var job = schedule.scheduleJob(date,function(){
	console.log("执行任务！！！");
});
// 取消任务
job.cancel();
```
###确定分钟
```js
var rule = new schedule.RecurrenceRule();
rule.minute = 42;

var job = schedule.scheduleJob(rule,function(){
	console.log('执行任务!!!');
})
```
###星期中某特定日期
```js
var rule = new schedule.RecurrenceRule();
rule.dayOfWeek = [0,new shcedule.Range(4,6)]; // 周四到周日
rule.hour = 17;
rule.minute = 0;
var j = schedule.scheduleJob(rule,function(){
	console.log('执行任务');
});
```
###每秒
- 可试用在分钟、小时上
```js
var times = [];
　　for(var i=1; i<60; i++){
　　　　times.push(i);
　　}
rule.second = times;
var c=0;
var j = schedule.scheduleJob(rule, function(){
	c++;
    console.log(c);
});
```