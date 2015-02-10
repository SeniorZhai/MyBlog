title: Express-Session
date: 2015-02-09 14:10:28
categories: node.js
tags: [session]
---
Session代表服务器与浏览器的一次会话过程，这个过程是连续的，也可以使时断时续的。
<!--more-->
express-session在Express中用于管理Seesion

```js
var session = require('express-session');	// 导入session模块

app.use(session({
	secret:settings.cookieSecrect,                      // 防止篡改cookie
    key:settings.db,                                    // cookie的名字
    cookie:{maxAge: 1000 * 60 * 60 * 24 *30},           // 30天
    store: new MongoStore({                             // MongoStore实例用来把会话信息存储到数据库中
        db:settings.db,
        host:settings.host,
        port:settings.port
    })
}));

app.get('/',function(req,res) {
	req.session.xx // 获取session中的引用
})
```