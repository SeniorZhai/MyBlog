title: node核心模块（一）
date: 2015-01-17 13:33:12
categories: node.js
tags: [HTTP]
---
<!--more-->
##全局对象
Node.js中的全局对象是`global`，所有全局变量都是`global`的属性
###process
process是一个全局变量，用于描述当前Node.js进程状态，提供了一个与操作系统的简单接口
- process.argv是命令行参数的数组
- process.studout 标准输出流
- process.stdin 标准输入流，初始化时它使被暂停的，必须恢复读取数据，并手动编写流的时间响应函数
```js
process.stdin.resume()
process.stdin.on('data',function(data){
	process.stdout.write('read from console: ' + data.toString());
});
```
- process.nextTick(callback) 为事件循环设置一项任务，Node.js会在下次时间循环响应时调用callback
```js
function doSomething(args,callback) {
	somthingComplicated(args);
	callback();
}

doSomething(function onEnd(){
	compute();
});
```
假设上面的代码中`somthingComplicated()`和`compute()`函数都是较为耗时的函数，上面的代码会先执行`somthingComplicated()`立即执行`compute()`
```js
function doSometing(args,callback) {
	somthingComplicated(args);
	process.nextTick(callback);
}

doSomething(fucntion onEnd(){
	compute();
})
```
###console
console用于控制台的标准输出
- console.log() 向标准输出流打印字符串以及换行符，也可以接受C语言printf()命令的格式输出
- console.error() 向标准错误流输出
- console.trace() 向标准错误流输出当前的调用栈
##常用工具util
util是Node.js核心模块，提供了常用函数的集合，用于弥补核心JavaScript的功能过于精简的不足
###util.inherits
实现对象原型继承的函数
```js
var util = require('util');

function Base() {
	this.name = 'base';
	this.base = 1991;

	this.sayHello = function() {
		console.log('Hello' + this.name);
	}

	this.prototype.showName = function() {
		console.log(this.name);
	}
};

function Sub() {
	this.name = 'sub';
}

util.inherits(Sub,Base);
var obj = new Sub()
obj.showName
console.log(obj)
```
	注；Sub仅仅继承了Base在原型中定义的函数，内部创造的base和sayHeloo都没有被继承
###util.inspect
util.inspect(object,[showHidden],[depth],[colors])是将任意对象转换成字符串的方法
可选参数showHideen控制是否输出更多隐藏信息
depth表示最大递归层数，默认递归2层，指定为null表示不限层数完整递归
color控制是否用ANSI颜色编码输出
##事件驱动events
`events`是Node.js最终要的模块，原因是Node.js本身的架构就是事件式的
###事件发射器
`events`模块只提供一个对象`events.EventEmitter`。EventEmitter对事件发射和事件监听功能进行了封装，每个事件由事件名和若干个参数组成，事件名是一个字符串。对于每个事件，EventEmitter支持若干个事件监听器，当事件发送时注册的事件监听器被依次调用，事件参数作为回调函数参数传递
```js
var events = require('events');

var emitter = new events.EventEmitter();

// 注册`someEvent`事件监听
emitter.on('someEvent',function(arg1,arg2){
	console.log('listner1',arg1,arg2);
})

emitter.on('someEvent',function(arg1,arg2){
	console.log('listner2',arg1,arg2);
})
// 发送事件
emitter.emit('someEvent','zoe',1991);
```
- EventEmitter.on(event,listener) 为指定事件注册一个监听器
- EventEmitter.emit(event,[arg1],[arg2],[...]) 发射`event`时间，可传递若干参数
- EventEmitter.once(event,listener) 为指定时间注册一个单次监听器，即监听器最多只会触发一次，触发后立即解除监听
- EventEmitter.removeListener(event,listener) 移除指定事件的某个监听器，listener必须是该事件已经注册过的监听器
- EventEmitter.removeAllListener([event]) 移除所有时间的所有监听器，如果指定event则移除指定时间的所有监听器
###error事件
EventEmitter定义了一个特殊的时间error，在遇到异常时会发射error，如果没有响应的监听器，Node.js会把它当做异常，退出程序打印调用栈。
```js
var events = require('events');

var emitter = new events.EventEmitter();

emitter.emitter('error');
```
###继承EventEmitter
很多时候，我们不会直接使用EventEmitter，而是在对象中继承它，包括`fs`，`net`，`http`在内的基于事件响应的核心模块都是EventEmitter的子类
##文件系统fs
`fs`模块是文件操作的封装，它提供了文件的读取、写入、更名、删除、遍历目录、链接等文件系统操作，fs还提供了异步和同步两个版本
###fs.readFile
fs.readFile(filename,[encoding],[callback(err,data)])是最简单的读取文件的函数，它接受一个必选参数filename表示要读取的文件名，第二个参数encoding表示文件的字符编码，callback是回调函数，err表示是否有错误，data是文件内容，如果没有指定encoding，data会是Buffer形式表示的二进制数据
```js
var fs = require('fs')

fs.readFile('error.js','utf-8',function(err,data){
	if(err){
		console.log(err)
	} else {
		console.log(data)
	}
})
```
###fs.readFileSync
fs.readFileSync是fs.readFile同步版本，读取到的数据会以返回值的形式返回，需要使用try和catch捕捉处理异常
###fs.open
`fs.open(path,flags,[mode],[callback(err,fd)])`，path为文件路径，flags可以使以下值
- r 以`读取模式`打开文件
- r+ 以`读写模式`打开文件
- w 以`写入模式`打开文件，如果文件不存在则创建
- w+ 以`读写模式`打开文件，如果文件不存在则创建
- a 以`追加模式`打开文件，如果不存在则创建
- a+ 以`读取追加模式`打开文件，如果不存在则创建
mode参数用于创建文件制定权限，默认是0666，回调函数将会传递一个文件描述符fd
###fs.read
`fs.read(fd,buffer,offset,length,postion,[callback(err,bytesRead,buffer)])`从制定的文件描述符fd中读取数据并写入buffer指向的缓冲区对象，offset是buffer的写入偏移量，length是要从文件中读取的字节数，position是文件读取的起始位置，如果position的值为null，则会从当前文件指针的位置读取，回调函数传递bytesRead和buffer分别表示读取的字节数和缓冲区对象
```js

```



