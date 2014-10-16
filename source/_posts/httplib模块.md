title: httplib模块
date: 2014-10-16 14:57:09
categories: Python
tags: [http,python]
---
<!--more-->
- httplib.HTTPConnection(host,[,port,[,strict,[,timeout]]])
	表示一次与服务器的交互，即请求/响应。host表示服务器主机，port表示端口号(默认值是80)，参数strict默认值为false，表示在无法解析服务器返回的状态行时(status line)是否剖出BadStatusLine异常，可选参数timeout表示超时时间
	+ HTTPConnection.request(method,url,[,body[,headers]])
		发送一次请求，method表示请求方法，url表示请求资源，body表示提交服务器的数据，必须是字符串，headers表示请求的http头
	+ HTTPConnection.getresponse()
		获取Http 响应
	+ HTTPConnection.connect()
		连接到HTTP服务器
	+ HTTPConnection.close()
		关闭与服务器连接
	+ HTTPConnection.set_debuglevel(level)
		设置调试级别，参数level默认为0，表示不输出任何调试信息
- httplib.HTTPResponse	表示服务器对客户端的相应，通过`HTTPConnection.getresponse()`来创建
	+ HTTPResponse.read([amt])
		获取响应的消息体，如果请求的是一个普通的网页，该方法返回页面的html，可选参数amt表示从响应流中读取指定字节的数据
	+ HTTPResponse.getheader(name[, default])
	　　获取响应头。Name表示头域(header field)名，可选参数default在头域名不存在的情况下作为默认值返回。
	+ HTTPResponse.getheaders()
	　　以列表的形式返回所有的头信息。
	+ HTTPResponse.msg
	　　获取所有的响应头信息。
	+ HTTPResponse.version
	　　获取服务器所使用的http协议版本。11表示http/1.1；10表示http/1.0。
	+ HTTPResponse.status
	　　获取响应的状态码。如：200表示请求成功。
	+ HTTPResponse.reason
　　 	返回服务器处理请求的结果说明。一般为”OK”

```python
headers = {"Content-type": "application/x-www-form-urlencoded"
                    , "Accept": "text/plain"}
values = {'param1':'2','param2':'3'}
params = urllib.urlencode(values)
conn = httplib.HTTPConnection("ave.bolyartech.com") 
conn.request("POST", "/params.php", params,headers)
response = conn.getresponse()    
print response.status, response.reason  
print response.read()
conn.close();
```