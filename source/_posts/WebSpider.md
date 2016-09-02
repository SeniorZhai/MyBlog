title: WebSpider
date: 2014-11-19 09:56:54
categories: Python
tags: [Spider,爬虫]
---
<!--more-->
## URI和URL

### URI(Universal Resource Identifier)是通用资源标志符
通常由三部分组成：
1. 访问资源的命名机制
2. 存放资源的主机名
3. 资源自身的名称
`http://www.baidu.com/img/bdlogo.png`可以解释为
1. 一个可以通过HTTP协议访问的资源
2. 位于主机`www.baidu.com`上
3. 通过`/img/bdlogo.png`访问

### URL
URL是URI的一个子集，是Uniform Resource Locator的缩写，译为__统一资源定位符__，简而言之URL是Internet上描述信息资源的字符串。
URL的一般格式为(带方括号[]为可选项)
`protocol://hostname[:port]/path/[parameters][?query]# fragment`
1. 第一部分是协议
2. 第二部分为存有资源的主机IP也可能包括端口号
3. 第三部分是主机资源的具体地址，如目录和文件名等

### 区别
URI属于URL更低级层次的抽象，一种字符串文本标准
URI表示请求服务器的路径定义一个资源，而URL同时说明要如何访问这个资源

## 抓取网页内容
```python
import urllib2
response = urllib2.urlopen('http://www.baidu.com')
html = response.read()
print html
```
使用请求
```python
import urllib2
req = urllib2.Request('http://www.baidu.com')
response = urllib2.urlopen(req)
the_page = respense.read()
print the_page
```
Post请求
```python
import urllib
import urllib2

url = 'http://www.someserver.com/register.cgi'

values = {'name':'WHY',
			'location':'SDU',
			'language','Python'}

data = urllib.urlencode(values)
req = urllib2.Request(url,data)
response = urllib2.urlopen(req)
the_page = response.read()
```
GET请求
```python
import urllib2
import urllib

data = {}

data['name'] = 'WHY'
data['location'] = 'SDU'
data['language'] = 'Python'

url_values = urllib.urlencode(data)
print url_values

name = Somebody + Here&language=Python&location=Northampton
url = 'http://www.example.com/example.cgi'
full_url = url + '?' + url_values

data = urllib2.open(full_url)
```
设置Headers
```python
import urllib
import urllib2

url = 'http://www.someserver.com/cgi-bn/register.cgi'

user_agent = 'Mozilla/4.0 (compatible; MSIE 5.5;Windows NT)'
values = {'name':'WHY',
		'location':'SDU',
		'language':'Python'}
headers = {'User-Agent':user_agent}
data = urllib.urlencode(values)
req = urllib2.Request(url,data,headers)
response = urllib.urlopen(req)
the_page = response.read()
```
## 异常
### URLError
通常，URLError在没用网络连接(没有路由到特定服务器)或者服务器不存在产生
这种情况下，异常同样会带有`reason`属性，他是一个tuple
包含了一个错误号和一个错误信息
```python
import urllib2

req = urllib2.Request('http://www.baibai.com')

try: urllib2.urlopen(req)

except urllib2.URLError,e:
	print e.reason
```
### HTTPError
服务器上每个HTTP应答对象response包含一个数字"状态码"
有时状态码服务器无法完成请求，默认的处理器会为你处理一部分应答
HTTP状态码分别以1~5开头由3位整数组成：
- 200:请求成功，处理方式：获得相应的内容，进行处理
- 201:请求完成，结果是创建了新资源，心创建资源的URI可再响应的实体中得到
- 202:请求被接受，处理尚未完成 处理方式：阻塞等待
- 204:服务器已经实现了请求，但没有返回心的信息，如果客户是用户代理，则无须为此更新自身的文档视图，处理方式 丢弃
- 300: 作为3XX类型回应的默认解释，存在多个可用的被请求资源。处理方式：若程序中能够处理，则进行进一步处理，如果程序不能处理，则丢弃
- 301:请求到飞资源都会被分配一个永久的URL，这个就可以在将来通过URL来访问此资源，处理方式：重定向到分配的URL
- 302:请求到的资源在一个不同的URL处临时保存，处理方式：重新向到临时的URL
- 400:非法请求 处理方式 丢弃
- 401:未授权 处理方式 丢弃
- 403:禁止 处理方式 丢弃
- 404:没有找到 处理方式 丢弃
- 5XX:服务器错误，处理方式 丢弃
===
HTTPError产生后会有一个整形`code`属性，是服务器发送的行管错误
BaseHTTPServer.BaseHTTPRequestHandler.reponse是一个很有用的应答号码字典，显示HTTP协议使用所有的应答号
```python
import urllib2
req = urllib2.Request('http://bbs.csdn.net/callmewhy')

try:
	urllb2.urlopen(req)
except urllib2.URLError, e:
	print e.code
```

