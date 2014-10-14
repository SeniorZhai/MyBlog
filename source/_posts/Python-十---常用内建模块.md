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
