title: JS操作JSON
date: 2015-02-09 16:38:55
categories: node.js
tags: [js,json]
---
JSON本身是JavaScript原生格式，这意味着JS操作JSON起来比其他语言都要优雅。
<!--more-->
JSON的结构一般分为`对象`和`数组`
对象以`{`和`}`包括，例如`{'a':'a','b':1234,c:'2008-08-08'}`
数组以`[`和`]`包括，例如`[{'a':'a','b':1234,c:'2008-08-08'},{'a':'a','b':1234,c:'2008-08-08'}]`
## 字符串转对象
```js
var obj = eval('(' + str + ')');	// 无论转多少次都是JSON对象
var obj = str.parseJSON();	// 会抛出异常
var obj = JSON.parse(str);
```
## 对象转字符串
```js
var str = obj.toJSONString();
var str = JSON.stringify(obj);
```

