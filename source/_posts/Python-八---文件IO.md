title: Python 八 - 文件IO
date: 2014-10-11 11:29:34
categories: Python
tags: [Python]
---
<!--more-->
##文件读写
Python内置的`open()`函数可以传入文件名和标示符打开一个文件对象
```python
f = open('./test.txt','r')
```
`r`表示读，如果文件不存在`open()`函数会抛出一个`IOError`错误
如果文件打开成功，调用`read()`函数可以一次读取文件的全部内容，Python把内容读到内存，用一个`str`对象表示
```python
str = f.read()
```
因为文件对象会占用系统资源，所以使用完毕需要调用`close()`函数关闭文件
Python引入`with`语句帮助我们自动调用`close`方法
```python
with open('./file','r') as f:
	f.read()
```
`read`方法可以`read(size)`每次最多读取size个字节的内容，调用`readline()`可以每次读取一行内容，调用`readlines()`一次读取所有内容并按行返回`list`
```python
for line in f.readlines():
	print(line.strip())	# 把末尾的`\n`删掉
```
###二进制文件
读取二进制文件比如图片、视频等，用`rb`模式打开即可：
```python
f = open('./test.jpg','rd')
```
###字符编码
读取非ASCALL编码的文本文件，必须以二进制模式打开，再解码，比如GBK编码的文件
```python
f = open('./gbk.txt','rb')
u = f.read().decode('gbk')
print u
```
##写文件
在`open`函数传入标识符`W`或者`wb`表示写文件或写二进制文件
```python
f = open('./test.txt','w')
f.write('Hello,world!')
f.close
```
也可以使用`with`语句
```python
with open('./test.txt','w') as f:
	f.write('Hello,world!')
```
写入特定编码也需要转码
##文件操作
要操作文件需要导入Python内置的`os`模块
```python
import os
os.name # 操作系统的名字
```
如果是`posix`说明系统是`Linux`、`Unix`或者`Mac OS X`，如果是`nt`就是`Windows`系统
获取系统详细信息可以调用`uname()`函数
通过`os.environ`可以看到系统环境变量的`dict`
要获取环境变量的某个值可以调用`os.getenv()`函数
###操作文件和目录
- os.path.abspath('.')	# 当前目录的绝对路径
- os.path.join('/User/zoe','testDir') # 合并路径
- os.path.split('/Users/zoe/testDir/file.txt') # 拆分目录
- os.path.splitext('/path/file.txt') # 拆分文件扩展名
- os.mkdir('./testdir') # 创建目录
- os.rmdir('./testdir') # 删除目录
- os.rename('test.txt','test.py') # 重命名
- os.remove('test.txt') # 删除文件
os模块中并没有复制文件可以使用`shutil`模块提供的`copyfile()`函数
```python
# 列出当前目录下的所有子目录
list1 = [x for x in os.listdir('.') if os.path.isdir(x)]
# 列出所有py文件
list2 = [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
```
##序列化
Python提供了`cPickle`和`pickle`两个模块来实现序列化，两个模块功能一致，区别在于`cPickle`是C语言写的，速度快，`pickle`是纯Python写的，速度慢，用的时候先尝试导入`cPickle`，如果失败了，再导入`pickle`
```python
try:
	import cPickle as pickle
except ImportError:
	import pickle
```
使用`pickle.dumps()`方法把任意对象序列化成一个str，然后，把str写入文件
也可以使用`pickle.dump()`直接把对象序列化写入file
```python
d = dict(name='Bob',age=20,score=88)
str = pickle.dumps(d)
f = open('dump.txt','wb')
pickle.dump(d,f)
f.close()
```
反序列化是用`pickle.loads()`方法通过读取到字符串反序列化出对象，也可以直接用`pickle.load()`方法从file中直接反序列化出对象
```python
f = open('dump.txt','rb')
d = pickle.load(f)
f.close()
```
Pickle的缺点很明显，只能用于Python并且可能不同版本的Python批次都不兼容
###JSON
JSON表示的对象就是标准的JavaScript语言的对象，JSON和Python内置的数据类型对应如下：

|JSON类型|Python类型|
|:-------|:---------|
|{}|dict|
|[]|list|
|"string"|str或u"unicode"|
|1234.56|int或float|
|true/flase|True/False|
|null|None|

Python内置的`json`模块提供了完善的Python对象到JSON格式的转换
```python
import json
d = dict(name='Bob',age=20,score=88)
json.dumps(d) # '{"age": 20, "score": 88, "name": "Bob"}'
```
同样`dump()`方法可以直接把JSON写入文件，`loads()`或`load()`方法可以把反序列化
###JSON进阶
对象序列化，需要实现一个转换函数
```python
import json
class Stundent(object):
	def __init__(self,name,age,score):
		self.name = name
		self.age = age
		self.score = score

s = Stundent('Bob',20,88)

def student2dict(std):
	return {
		'name':std.name,
		'age':std.age,
		'score':std.score
	}

json.dumps(s,default=student2dict) # 先被student2dict转换成dict在顺利序列化为JSON
```
通常`class`实例都有一个`__dict__`属性，用来存储实例变量，所以也可以这么写：
```python
json.dumps(s,default=lambda obj:obj.__dict__)
```
反序列化时
```python
def dict2student(d):
	return Student(d['name'],d['age'],d['score'])

json.loads(json_str,object_hook=dict2student)
```