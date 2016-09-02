title: Express学习笔记-快速开始
date: 2015-01-20 11:45:05
categories: node.js
tags: [Express学习笔记]
---
express是`Node.js`上最流行的Web开发框架，使用它可以快速的开发一个 Web 应用。
<!--more-->
## 安装Express
在命令行工具中下载Express
```shell
$ npm install -g express-generator
```
## 新建工程
```shell
$ express -e blog ##  新建工程 使用ejs模板引擎
$ cd blog && npm install ##  安装依赖
```
## 启动项目
```shell
DEBUG=blog node ./bin/www
```
## 工程结构
```
- app.js 			#  启动文件
- bin/				#  存储工程信息及模块依赖
- node_modules/		#  存放package.json中安装的模块，添加依赖的模块并安装后，存放在这个文件夹下
- package.json 		#  存放image、css、js等文件
- public			#  存放路由文件
- routes			#  存放视图文件
- views				#  存放可执行文件
```
### 分析app.js
```js
var app = express();	// 生成express实例
app.set('views',path.join(__dirname,'views'));	// 设置views文件夹为存放视图文件的目录，__dirname为全局变量，存放当前正在执行的脚本所在的目录
app.set('view engine','ejs');	// 设置模板引擎为ejs

// app.use(favicon(__dirname + '/public/favicon.ico')); 	// 设置favicon图标
app.use(logger('dev')); 	// 加载日志中间件
app.use(bodyParser.json()); 	// 加载解析JSON的中间件
app.use(bodyParser.urlencoded({extended:false}));	// 加载urlencoded请求的中间件
app.use(cookieParser());	// 加载解析cookie的中间件
app.use(express.static(path.join(__dirname,'public')));	// 设置public文件夹为静态文件的目录
app.use('/',routes);
app.use('/users',users); 	// 路由控制器
app.use(function(req,res,next){	// 捕获404错误
	var err = new Error('Not Found');
	err.status = 404;
	next(err);
});	
if (app.get('env') === 'development') {	// 开发环境中，将错误信息直接渲染error模板并显示到浏览器
	app.use(function(err,req,res,next){
		res.status(err.status || 500);
		res.render('error',{
			message:err.message,
			error:err
		})
	})
}
app.use(function(err,req,res,next){	// 生产环境下，将错误信息选人成Error模板显示
	res.status(err,status||500);
	res.render('err',{
		message:err.message,
		error:{}
	});
});

module.exports = app; // 导出app实例供其他模块调用
```
### bin/www文件
```js
# ! /usr/bin/env node 	// 表明是node可执行文件
var debug = require('debug')('blog')	// 引入debug模块打印调试日志
var app = require('../app');	// 引入app实例

app.set('port',process.env.PORT || 3000);	// 设置端口号

var server = app.listen(app.get('port'),function(){	// 启动工程并监听3000端口
	debug('Express server listening on port ' + server.address().port);
});
```
### routes/index.js
```js
var express = require('express');
var router = express.Router();

router.get('/',function(req,res){
	res.render('index',{title:'Express'}); // 渲染
})

module.exports = router;
```
### views/index.ejs
```html
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
  </body>
</html>
```
渲染模板时，传入的title值为`Express`字符串，模板引擎会将所有<%= title %>替换为express

## 路由控制
在`routes/index.js`中
```js
router.get('/',function(req,res){
	res.render('index',{title:'Express'});
});
```
index.js问价就是用于管理app的路由，简单的路由分配
为了实现路由的统一管理，可以把所有路由控制器和实现路由功能的函数都放在`index.js`里
- 在app.js中修改
```js
// 删除app.use('/', routes);  app.use('/users', users);
var routes = reuire('./routes/index');
// ...
// 去除所有路由控制的代码
routes(app);

app.listen(app.get('port'), function() {
  console.log('Express server listening on port ' + app.get('port'));
});
```
- 修改index.js
```js
module.exports = function(app) {
	app.get('/',function(req,res){
		res.render('index',{title:'Express'});
	});
};
```
## 路由规则
`express`封装了多种http请求方式，主要使用`get`和`post`两种，即`app.get()`和`app.post()`
这两个函数第一个参数都为请求的路径，第二个参数为处理请求的回调函数，回调函数的两个参数分别是req和res，表示请求信息和响应信息
路径请求及对应的获取路径有以下几种形式
### req.query
处理GET请求，获取GET请求参数
```js
// GET /search?q=tobi+ferret
req.query.q // "tobi ferret"
// GET /shoes?oder=desc&shoe[color]=blue&shoe[type]=converse
req.query.order // desc
req.query.shoe.color // blue
req.query.shoe.type // converse
```
### req.body
获取post请求，获取post请求
```js
// POST user[name]=tobi&user[email]=tobi@learnboost.com
req.body.user.name // tobi
req.body.user.email // tobi@learnboost.com
```
### req.params
处理`/:xxx`形式的get或post请求，获取请求参数
```js
// GET /user/tj
req.params.name // tj
// GET /file/javascripts/jquery.js
req.params[0] // javascripts/jquery.js
```
### req.param(name)
处理get和post请求，但查找的优先级由高到低为`req.parms->req.body->req.query`
```js
// GET ?name=tobi
req.param('name')	// tobi
// POST name=tobi
req.param('name')	// tobi
// GET /user/tobi for /user/:name
req.param('name') 	// tobi
```
## 添加路由规则
修改index.js，在app.get('/')函数后添加一条路由规则
```js
app.get('/new',function(req,re))
```




















