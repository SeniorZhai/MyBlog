title: Python实现简单的Post请求
date: 2014-10-13 15:19:52
categories: Python
tags: [Python,Post]
---
<!--more-->
```python
import urllib
import urllib2

url = 'http://ave.bolyartech.com/params.php'
values = {'param1':'2','param2':'3'}
data = urllib.urlencode(values)
print data
req = urllib2.Request(url, data)	#  Post请求
response = urllib2.urlopen(req)
the_page = response.read()	#  读取返回数据
print the_page
```