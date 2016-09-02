title: node核心模块（二）
date: 2015-01-17 13:33:12
categories: node.js
tags: [HTTP]
---
Node.js标准库提供了http模块，其中封装了一个高效的HTTP服务器和简易的HTTP客户端。http.Server是一个基于事件的HTTP服务器，核心由C++部分实现，接口由JavaScripte封装，兼顾了高性能和简易性
http.request则是一个HTTP客户端工具，用于HTTP服务器发起请求
<!--more-->
## HTTP服务器
http.Server是http模块中的HTTP服务器对象
```js
var http = require('http')
http.createServer(function(req,res){
	res.writeHead(200,{'Content-Type':'text/html'});
	res.write('<h1>Node.js</h1>');
	res.end("<p>Hello World</p>");
}).listen(3000);
```
### http.Server的事件
- request 客户端请求到来时该事件被触发，提供两个参数req和res，分别是`http.ServerRequest`和`http.ServerResponse`的实例，表示请求和响应信息
- connection 当TCP链接建立时，该事件被触发，提供了一个参数socket，为`net.Socket`的实例
- close 当服务器关闭时，该事件被触发
此外还有`checkContinue`、`upgrade`、`clientError`事件，通常不需要关心
最常用到的时`request`，为此http提供了`http.createServer([requestListener])`，功能是创建HTTP服务器并将`requestListener`作为request事件的监听函数
```js
var http = require('http')

var server = new http.Server();
server.on('request',function(req,res){
	res.writeHead(200,{'Content-Type':'text/html'});
	res.write('<h1>Node.js</h1>');
	res.end("<p>Hello World</p>");
});
server.listen(3000);
```
### http.ServerRequest
http.ServerRequest是HTTP请求的信息，是后端开发者最关注的内容，一般由http.Server的request事件发送，作为参数，通常简称`req`或`request`
HTTP请求一般分为两部分:请求头(RequestHeader)和请求体(RequestBody)，http.ServerRequest提供了3个事件用于控制请求体的传输
- data 当请求体数据到来时，事件触发，该事件提供了一个参数`chunk`，表示接收到的数据，如果该事件没有被监听，那么请求体会被抛弃
- end 当请求体传输完成时，该事件被触发，此后将不会再有数据到来
- close 用户请求结束时，该事件被触发

ServerRequest的属性
|名称|含义|
|:--|:---|
|complete|客户端请求是否已经发送完成|
|httpVersion|HTTP协议版本，通常是1.0或1.1|
|method|HTTP请求方法，如GET、POST、PUT、DELETE等|
|url|原始的请求路径|
|headers|HTTP请求头|
|trailers|HTTP请求尾(不常见)|
|connection|当前HTTP链接套接字，为net.Socket的实例|
|socket|connection属性的别名|
|client|client属性的别名|
### 获取GET请求
```js
var http = require('http')
var url = require('url')
var util = require('util')
http.createServer(function(req,res){
	res.writeHead(200,{'Content-Type':"text/plain"});
	res.end(util.inspect(url.parse(req.url,true)));
}).listen(3000);
```
### 获取POST请求
```js

```