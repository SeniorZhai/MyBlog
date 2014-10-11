title: Python 七 - 错误与调试
date: 2014-10-11 11:28:31
categories: Python
tags: [Python]
---
<!--more-->
##try错误
```python
try:
	r = 10/0
	print r
except ZeroDivisionError,e:
	print 'except:',e
finally:
	print 'finally'
print 'END'
```
在某段代码可能会出错时，就可以用`try`来运行这段代码，如果执行出错，则后续代码不会继续执行，而是直接跳转至错误处理代码，即`except`语句块，执行完`except`后，如果有`finally`语句块，则执行`finally`语句块，至此执行完毕
如果没有发生错误发生，可以在`except`语句块后面加一个`else`，如果没有错误发生时，会自动执行`else`语句

##记录错误
Python内置的`logging`模块可以非常容易地记录错误信息：
```python
# err.py
import logging

def foo(s):
	return 10 / int(s)

def bar(s):
	return foo(s) * 2

def main():
	try:
		bar('0')
	except StandardError,e:
		logging.exception(e)

main()
print 'END'
```
同样是出错，但程序打印完错误后会继续执行，并正常退出
##抛出错误
错误是class，捕获一个错误就是捕获到该class的一个实例，因此，错误不是凭空产生的，而是有意创建抛出的。Python内置函数会抛出很多类型的错误，可以使用`raise`语句抛出错误
```python
# err.py
class FooError(StandarError):
	pass

def foo(s):
	n = int(s)
	if n == 0:
		raise FooError('invalid value:%s' % s)
	return 10/n
```

##断言
```python
# err.py
def foo(s):
	n = int(s)
	assert n != 0,'n is zero!'
	return 10 / n

def main():
	foo('0')
```
`assert`的意思是，表达式`n != 0`应该是`True`，否则，后面的代码就会出错(AssertionError)
Python解释器可以用`-0`参数来关闭`assert`

##logging
```python
# err.py
import logging

s = '0'
n = int(s)
logging.info('n = %d' % n)
print 10 / n
```
`logging.info()`可以输出一段文本，可以使用`logging.basicConfig(level=logging.INFO)`指定记录信息的级别，有`debug`、`info`、`warning`、`error`，当指定`level=INFO`时，其他级别都不起作用，其他同理。

##pdb
调试器，可以让程序以单步方式运行，可以随时查看运行状态
```python
# err.py
s = '0'
n = int(s)
print 10 / n
```
然后启动
	$ python -m pdb err.py
以参数`-m pdb`启动后，pdb定位到下一步要执行的代码`-> s = '0'`输入命令`l`查看运行代码
输入`n`可以单步指定代码
也可以输入`p 变量名`来查看变量
输入`q`结束调试，退出程序

##pdb.set_trace()
只需要`import pdb`然后在可能出错的地方放一个`pdb.set_trace()`就可以设置一个断点
```python
# err.py
import pdb

s = '0'
n = int(s)
pdb.set_trace()
print 10 / n
```
运行代码，程序自动在`pdb.set_trace()`暂停并进入pdb调试环境，可以用`c`继续运行
