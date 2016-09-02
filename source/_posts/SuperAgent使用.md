title: SuperAgent使用
date: 2015-05-13 11:12:25
categories: node.js
tags: [http,爬虫,cookie,访问]
---
[SuperAgent](https://github.com/visionmedia/superagent)是一个非常方便的客户端请求模块，能方便的发起get、post、put、delete、head请求
<!--more-->
## 简单使用
```js
request
	.get('/')
	.end(function(res){
		// 处理数据
	});
```
可以使用参数传递
```js
request('GET','/search').end(callback);
```
也可以使用绝对路径
```js
request.get('http://example.com/search')
	.end(function(res))
```
`delete`、`head`、`post`、`put`等HTTP动作都可以使用
```js
request
	.head('/favicon.ico')
	.end(function(res){
		//
	})
```
`delete`是默认的保留字所以使用`.del()`来指定delete动作
```js
request
	.del('/user/1')
	.end(function(res){
		//
	});
```
## 设置头字段
只需要调用`.set()`方法即可
```js
request
	.get('/search')
	.set('API-Key','foobar')
	.set('Accept','application/json')
	.end(callback);
```
也可以传递一个对象，一次修改多个字段
```js
 request
   .get('/search')
   .set({ 'API-Key': 'foobar', Accept: 'application/json' })
   .end(callback);
```
## GET请求
使用`.query()`方法可以传递一个对象完成请求参数
```js
reques
	.get('/search')
	.query({query:'Manny'})
	.query({rang:'1.5'})
	.end(function(res){
		//
	});
```
也可以合并成一个对象
```js
request
	.get('/search')
	.query({query:'Manny',range:'1.5'})
	.end(function(res){
		//
	});
```
也可以直接传递字符串
```js
request
	.get('/search')
	.query('query=Manny&range=1.5')
	.end(function(res){
		//
	});
```
或者字符串拼接
```js
request
    .get('/querystring')
    .query('search=Manny')
    .query('range=1..5')
    .end(function(res){
    	//
    });
```
## POST/PUT请求
- POST Json数据
```js
request.post('/user')
	.set('Content-Type','application/json')
	.send('{"name":"tj","pet":"tobi"}')
	.end(callback);
```
默认支持`form`和`json`两种格式，不设置Content-Type也是可以的
- 以`application/x-www-form-urlencoded`类型发送数据，调用`.type()`方法传递form参数即可
```js
request.post('/user')
    .type('form')
    .send({ name: 'tj' })
    .send({ pet: 'tobi' })
    .end(callback)
```
> form是form-data和urlencoded的别名

## 设置Content-Type
```js
request.post('/user')
	.type('application/json')

request.post('/user')
	.type('json')

request.post('/user')
	.type('png')
```

## 设置接收类型
```js
request.get('/user')
	.accept('application/json')

request.get('/user')
	.accept('json')

request.get('/user')
	.accept('png')
```

## 查询字符串
当POST请求并希望传递一些查询字符串时，可以同时使用`.query()`方法
```js
request
  .post('/')
  .query({ format: 'json' })
  .query({ dest: '/login' })
  .send({ post: 'data', here: 'wahoo' })
  .end(callback);
```

## 解析响应内容
SuperAgent会解析一些常用的格式，当前支持`application/json`，`application/x-www-form-urlencoded`，`multipart/form-data`

### JSON/Urlencoded
res.body是解析后的内容对象，比如一个请求响应`'{“user:{"name":"tobi"}}'`字符串，res.body.user.name将会返回tobi

### Multipart
res.files属性就可以直接获取文件

## 响应属性
返回为response对象

### text
res.text为未解析的响应内容，一般只在mime类型能够匹配`text`，`json`，`x-www-form-urlencoding`的情况

### body
当Content-Type定义一个解析器后，自动解析，默认解析`application/json`和`application/x-www-form-urlencoding`

### header
res.header包含解析之后的响应投数据，键值都是node处理成小写字幕形成，比如`res.header['content-lenth']`

### Content-Type
Content-Type响应头字段的一个特例，服务器提供res.type来访问他，默认res.charset。
如Content-Type为`text/html;charset=utf-8`则res.type为text/html，res.charset为utf-8

### Status
响应状态标志位
```js
var type = status / 100 | 0;

 // status / class
 res.status = status;
 res.statusType = type;

 // basics
 res.info = 1 == type;
 res.ok = 2 == type;
 res.clientError = 4 == type;
 res.serverError = 5 == type;
 res.error = 4 == type || 5 == type;

 // sugar
 res.accepted = 202 == status;
 res.noContent = 204 == status || 1223 == status;
 res.badRequest = 400 == status;
 res.unauthorized = 401 == status;
 res.notAcceptable = 406 == status;
 res.notFound = 404 == status;
 res.forbidden = 403 == status;
```

## 中止请求
`req.abort()`可以中止请求

## 请求超时
`req.timeout()`来定义超时时间

## 基础验证
1. 传递URL
```js
request.get('http://tobi:learnboost[@local](/user/local)').end(callback);
```
2. 调用`.auth()`方法
```js
request
  .get('http://local')
  .auth('tobo', 'learnboost')
  .end(callback);
```

### 重定向
可以通过调用.res.redirects(n)来设置个数，默认是5个
```js
request
  .get('/some.png')
  .redirects(2)
  .end(callback);
```

### 管道数据
允许使用一个请求流来输送数据,比如请求一个文件作为输出流
```js
var request = require('superagent')
  , fs = require('fs');

var stream = fs.createReadStream('path/to/my.json');
var req = request.post('/somewhere');
req.type('json');
stream.pipe(req);

// 保存到文件
var request = require('superagent')
  , fs = require('fs');

var stream = fs.createWriteStream('path/to/my.json');
var req = request.get('/some.json');
req.pipe(stream);
```

### 复合请求
```js
var req = request.post('/upload');

req.part()
   .set('Content-Type', 'image/png')
   .set('Content-Disposition', 'attachment; filename="myimage.png"')
   .write('some image data')
   .write('some more image data');

req.part()
   .set('Content-Disposition', 'form-data; name="name"')
   .set('Content-Type', 'text/plain')
   .write('tobi');

req.end(callback);
```

## 附加文件
可以通用.attach(name, [path], [filename])和.field(name, value)这两种形式来调用.添加多个附件也比较简单，只需要给附件提供自定义的文件名称,同样的基础名称也要提供.
```js
request
  .post('/upload')
  .attach('avatar', 'path/to/tobi.png', 'user.png')
  .attach('image', 'path/to/loki.png')
  .attach('file', 'path/to/jane.png')
  .end(callback);
```

### 字段值
跟html的字段很像,你可以调用.field(name,value)方法来设置字段,假设你想上传一个图片的时候带上自己的名称和邮箱，那么你可以像下面写的那样:
```js
 request
   .post('/upload')
   .field('user[name]', 'Tobi')
   .field('user[email]', 'tobi[@learnboost](/user/learnboost).com')
   .attach('image', 'path/to/tobi.png')
   .end(callback);
```

### 缓冲响应
为了强迫缓冲res.text这样的响应内容,可以调用req.buffer()方法,想取消默认的文本缓冲响应像text/plain,text/html这样的，可以调用req.buffer(false)方法
当缓冲res.buffered标识提供了，那么就可以在一个回调函数里处理缓冲和没缓冲的响应.

### 跨域资源共享
`.withCredentials()`方法可以激活发送原始cookie的能力,不过只有在Access-Control-Allow-Origin不是一个通配符(*),并且Access-Control-Allow-Credentials为`true`的情况下才行.
```js
request
  .get('http://localhost:4001/')
  .withCredentials()
  .end(function(res){
    assert(200 == res.status);
    assert('tobi' == res.text);
    next();
  })
```

### 异常处理
当发送错误时，superagent首先会检查回调函数的参数数量,当err参数提供的话，参数就是两个,如下:
```js
request
 .post('/upload')
 .attach('image', 'path/to/tobi.png')
 .end(function(err, res){

 });


 request
  .post('/upload')
  .attach('image', 'path/to/tobi.png')
  .on('error', handle)
  .end(function(res){
  	//
  });
```
> 注意:superagent默认情况下,对响应4xx和5xx的认为不是错误,例如当响应返回一个500或者403的时候,这些状态信息可以通过res.error,res.status和其它的响应属性来查看,但是没有任务的错误对象会传递到回调函数里或者emit一个error事件.正常的error事件只会发生在网络错误,解析错误等.

当产生一个4xx或者5xx的http错误响应,res.error提供了一个错误信息的对象，你可以通过检查这个来做某些事情.
```js
if (res.error) {
  alert('oh no ' + res.error.message);
} else {
  alert('got ' + res.status + ' response');
}
```
