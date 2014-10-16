title: Python-十 - 常用内建模块
date: 2014-10-13 11:17:22
categories: Python
tags: [Python]
---
collections是Python内建的一个集合模块，提供了许多有用的集合类。
<!--more-->
##namedtuple
`tuple`可以表示不变集合，例如一个二维坐标就可以表示成`p = (1,2)`
如果使用class表示太过臃肿，`namedtuple`就可以派上用场
```python
from collections import namedtuple
Point namedtuple('Point',['x','y'])
p = Point(1,2)
```
`namedtuple`是一个函数，用来创建自定义的`tuple`对象，并且规定了`tuple`元素的个数，并可以用属性而不是索引来引用`tuple`的某个元素
```python
# 定义一个圆
Circle = namedtuple('Circle',['x','y','r'])
```
##deque
`deque`可以高效地实现插入和删除操作的双向列表，适用于队列和栈
```python
from collections import deque
q = deque(['a','b','c'])
q.append('x')
q.appendleft('y')	# 添加到头部
```
`deque`除了实现list的`append()`和`pop()`外，还支持`appendleft()`和`popleft()`可以头部添加或删除元素
##defaultdict
适用`dict`时，如果引用的key不存在，就会抛出`KeyError`，如果希望不存在时，返回一个默认值，就可以用`defaultdict`:
```python
from collections import defaultdict
dd = defaultdict(lambda: 'N/A')
dd['key1'] = 'abc'
dd['key2']	# 'N/A'
```
##OrderedDict
`dict`的Key是无序的，`OrderedDict`的key会按照插入的顺序排列，不是Key本身的排序
```python
from collections import OrderedDict
od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
```
`OrderedDict`可以实现一个FIFO（先进先出）的dict，当容量超出限制时，先删除最早添加的Key：
```python
from collections import OrderedDict

class LastUpdatedOrderedDict(OrderedDict):

    def __init__(self, capacity):
        super(LastUpdatedOrderedDict, self).__init__()
        self._capacity = capacity

    def __setitem__(self, key, value):
        containsKey = 1 if key in self else 0
        if len(self) - containsKey >= self._capacity:
            last = self.popitem(last=False)
            print 'remove:', last
        if containsKey:
            del self[key]
            print 'set:', (key, value)
        else:
            print 'add:', (key, value)
        OrderedDict.__setitem__(self, key, value)
```
##Counter
`Counter`是`dict`的子类，是一个简单的计数器
```python
from collections import Counter
c = Counter()
for ch in 'programming':
	c[ch] = c[ch] + 1
c # Counter({'g': 2, 'm': 2, 'r': 2, 'a': 1, 'i': 1, 'o': 1, 'n': 1, 'p': 1})
```
##base64
Base64是一种用64个字符表示任意二进制数据的方法
Base64的原理很简单，首先准备一个包含64个字符的数组：
	['A', 'B', 'C', ... 'a', 'b', 'c', ... '0', '1', ... '+', '/']
每3个字节一组，一共`3x8=24bit`，划为4组，每组正好6bit:
	
![](/img/14101301.png)

这样的得到4个字数作为索引，然后查表，获得相应的4个字符，就是编码后的字符串
Base64编码会把3个字节的二进制数据编码为4字节的文本数据，长度增加33%，好处是编码后的文本数据可以在邮件正文、网页等直接显示
如果要编码的二进制数据不是3的倍数，最后会剩下1个或2个字节怎么办？Base64用\x00字节在末尾补足后，再在编码的末尾加上1个或2个=号，表示补了多少字节，解码的时候，会自动去掉
```python
import base64
base64.b64encode('binary\x00string')	# 'YmluYXJ5AHN0cmluZw=='
base64.b64decode('YmluYXJ5AHN0cmluZw==')# 'binary\x00string'
```
标准的Base64编码后可能出线`+`和`/`，在URL就不能直接作为参数，可以使用`url safe`的base编码，把字符`+`和`/`分别变成`-`和`_`
```python
base64.b64encode('i\xb7\x1d\xfb\xef\xff')	# 'abcd++//'
base64.urlsafe_b64encode('i\xb7\x1d\xfb\xef\xff')	# 'abcd--__'
base64.urlsafe_b64decode('abcd--__')	# 'i\xb7\x1d\xfb\xef\xff'
```
由于`=`字符会出现在`Base64`编码中，但`=`用在URL、Cookie里面会造成歧义，很多Base64编码后会把`=`去掉
因为Base64是把3个字节变成4个字节，所以Base64编码的长度永远是4的倍数，去掉`=`后，解码时增加`=`到4的倍数，再正常解码

##struct
Python的`struct`模块用来处理`str`和其他二进制数据类型的转换
```python
import struct
struct.pack('>I',10240099)	# '\x00\x9c@c' 把任意数据类型变成字符串
```
`pack`第一个参数是处理指令，`>`表示字节顺序是big-endian(网络序)，`I`表示4字节无符号整数
```python
struct.unpack('>IH','\xf0\xf0\xf0\xf0\x80\x80')	# (4042322160,32896)
```
`H`表示2字节无符号整数
Windows的位图文件（.bmp）是一种非常简单的文件格式，我们来用struct分析一下。
首先找一个bmp文件，没有的话用“画图”画一个。
读入前30个字节来分析：
```python
>>> s = '\x42\x4d\x38\x8c\x0a\x00\x00\x00\x00\x00\x36\x00\x00\x00\x28\x00\x00\x00\x80\x02\x00\x00\x68\x01\x00\x00\x01\x00\x18\x00'
BMP格式采用小端方式存储数据，文件头的结构按顺序如下：
```
两个字节：'BM'表示Windows位图，'BA'表示OS/2位图； 一个4字节整数：表示位图大小； 一个4字节整数：保留位，始终为0； 一个4字节整数：实际图像的偏移量； 一个4字节整数：Header的字节数； 一个4字节整数：图像宽度； 一个4字节整数：图像高度； 一个2字节整数：始终为1； 一个2字节整数：颜色数。
所以，组合起来用unpack读取：
```python
>>> struct.unpack('<ccIIIIIIHH', s)
('B', 'M', 691256, 0, 54, 40, 640, 360, 1, 24)
```
结果显示，'B'、'M'说明是Windows位图，位图大小为640x360，颜色数为24。

##hashlib
hashlib提供了常见的摘要算法，如MD5，SHA1等
摘要算法又称为哈希算法、散列算法，通过函数把任意长度的数据转换成为一个长度固定的数据串(通常用16进制的字符串表示)
摘要算法是单向的，反向推导十分困难，原始数据有一点改动，都会导致计算出的摘要完全不同
```python
import hashlib

md5 = hashlib.md5()
md5.update('how to use md5 in python hashlib?')
print md5.hexdugest()	# d26a53750bc40b38b65a520292f69306
# 如果数据量很大，可以多次调用update()
md5.update('how to use md5 in')
md5.update('python hashlib?')
```
MD5是最常见的摘要算法，速度很快，生成结果是固定的128bit字节，通常用一个32位的16进制字符串表示
常见的摘要算法还有SHA1:
```python
import hashlib

sha1 = hashlib.sha1()
sha1.update('how to use sha1 in ')
sha1.update('python hashlib?')
print sha1.hexdigest()
```
SHA1的结果是160bit字节，通常用一个40位的16进制字符串表示
比SHA1更安全的算法是SHA256和SHA512，不过越安全的算法越慢，而且摘要长度更长

###摘要算法的应用
比如用户的密码，使用明文保存十分的不安全，可以使用摘要算法保存摘要
一些常用口令的MD5值容易被计算出来，所以可以在用户口令基础上加复杂的字符串实现`加盐算法`
```python
def calc_md5(password):
	return get_md5(password + 'the-Salt')
```
只要Salt(盐)不泄露，即使再简单的MD5反推明文也是很难实现的

##XML
XML作为一种包装数据的形式，使用的范围越来越小，远远不及JSON，但是还是有必要了解
操作XML有两种方法：DOM和SAX，DOM会把整个XML读入到内存，因此占用内存大，解析慢，但是可以访问任意树的节点。SAX是流模式，边读边解析，占用内存小，解析快，但是要处理事件
```python
# 解析<a href="/">python</a>
from xml.parsers.expat import ParseCreate

class DefaultSaxHandler(object):
	def start_element(self,name,attrs):
		print('sax:start_element:%s,attrs:%s' % (name,str(attrs)))

	def end_elemnet(self,name):
		print('sax:end_element:%s' % name)

	def char_data(self,text):
		print('sax:char_data:%s' % text)

handler = DefaultSaxHandler()
parser = ParserCreate()
parser.returns_unicode = True	# 返回element名称和char_data都是unicode
parser.StartElementHandler = handler.start_element
parser.EndElementHandler = handler.end_element
parser.CharacterDataHandler = handler.char_data
parser.Parse(xml)
```

##HTMLParser
HTML是XML子集，语法不如XML严格，解析时可以使用HTMLParser
```python
from HTMLParser import HTMLParser
from htmlentitydefs import name2codepoint

class MyHTMLParser(HTMLParser):
	
	def handle_starttag(self,tag,attrs):
		print('<%s>' % tag)

	def handle_endtag(self,tag):
		print('</%s>' % tag)

	def handle_startendtag(self,tag,attrs):
		print('<%s/>' % tag)

	def handle_data(self,data):
		print('data')

	def handle_comment(self,data):
		print('<!-- -->')

	def handle_entityref(self,name):
		print('&%s;' % name)

	def handle_charref(self,name):
		print('&#%s;' % name)

parser = MyHTMLParser()
parser.feed('<html><head></head><body><p>Some <a href=\"#\">html</a> tutorial...<br>END</p></body></html>')
```
`feed`可以调用多次，不一定要一次性把HTML字符串都塞进去
