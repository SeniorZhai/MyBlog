title: Express基础使用
date: 2015-01-19 20:24:01
categories: node.js
tags: [express]
---
<!--more-->
#创建应用
```js
var express = require('express');
var app = express();
app.get('/',function(req,res){
	res.send('hello world');
})

app.listen(3000);
```
##app设置
###值设置
- app.set(name,value)
```js
app.set('title','My Site');
```
- app.get(name)
```js
app.get('title'); // 'My Site'
```

###选项设置
- app.enable(name)	// 设置某项为true
- app.disable(name)	// 设置某项为false，禁用
- app.enabled(name)	// 检查某项是否为true
- app.disabled(name) // 检查某项是否为false

###环境配置
app.configure([env],callback)
```js
// 所有环境
app.configure(function(){
  app.set('title', 'My Application');
})

// 开发环境
app.configure('development', function(){
  app.set('db uri', 'localhost/dev');
})

// 只用于生产环境
app.configure('production', function(){
  app.set('db uri', 'n.n.n.n/prod');
})
```
更高效且直接的代码如下(历史遗留问题)：
```
// 所有环境
app.set('title', 'My Application');

// 只用于开发环境
if ('development' == app.get('env')) {
  app.set('db uri', 'localhost/dev');
}

// 只用于生产环境
if ('production' == app.get('env')) {
  app.set('db uri', 'n.n.n.n/prod');
}
```

###使用中间件
app.use([path],function) path默认为"/"
```js
var express = require('express')
var app = express()
// 简单的logger
app.use(function(req,res,next){
	console.log("%s %s",req.method,req,url)
	next();
})

app.use(function(req,res,next){
	res.send('Hello World!')
});

app.listen(3000);
```

###setting
使用内建的方式改变Express行为的设置
- env 运行时环境，默认为 process.env.NODE_ENV 或者 "development"
- trust proxy 激活反向代理，默认未激活状态
- jsonp callback name 修改默认?callback=的jsonp回调的名字
- json replacer JSON replacer 替换时的回调, 默认为null
- json spaces JSON 响应的空格数量，开发环境下是2 , 生产环境是0
- case sensitive routing 路由的大小写敏感, 默认是关闭状态， "/Foo" 和"/foo" 是一样的
- strict routing 路由的严格格式, 默认情况下 "/foo" 和 "/foo/" 是被同样对待的
- view cache 模板缓存，在生产环境中是默认开启的
- view engine 模板引擎
- views 模板的目录, 默认是"process.cwd() + ./views"

##Request

##req.params
命名过的参数会以键值对的形式存放，默认是`{}`
```js
// GET /user/zoe 路由设置为/user/:name
req.params.name //zoe
```






