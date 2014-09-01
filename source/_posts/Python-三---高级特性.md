title: Python 三 - 高级特性
date: 2014-06-13 11:15:09
categories: Python
tags: [Python]
---
##切片
比如一个list去前3个元素,取指定索引范围的操作，用循环十分繁琐，因此，Python提供了切片（Slice）操作符，能大大简化这种操作。
```
>>> r = ['a','b','c','d','e','f','g']
>>> r[0:3]
['a', 'b', 'c']
```
`L[0:3]`表示，从索引0开始取，直到索引3为止，但不包括索引3。即索引0，1，2，正好是3个元素。
记住倒数最后一个元素的索引是-1。

切片操作十分有用。我们先创建一个0-99的数列：
```
>>> L = range(100)
>>> L
[0, 1, 2, 3, ..., 99]
```
可以通过切片轻松取出某一段数列。比如前10个数：
```
>>> L[:10]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
后10个数：
```
>>> L[-10:]
[90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
```
前11-20个数：
```
>>> L[10:20]
[10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
```
前10个数，每两个取一个：
```
>>> L[:10:2]
[0, 2, 4, 6, 8]
```
所有数，每5个取一个：
```
>>> L[::5]
[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]
```
甚至什么都不写，只写[:]就可以原样复制一个list：
```
>>> L[:]
[0, 1, 2, 3, ..., 99]
```
tuple也是一种list，唯一区别是tuple不可变。因此，tuple也可以用切片操作，只是操作的结果仍是tuple：
```
>>> (0, 1, 2, 3, 4, 5)[:3]
(0, 1, 2)
```
字符串'xxx'或Unicode字符串u'xxx'也可以看成是一种list，每个元素就是一个字符。因此，字符串也可以用切片操作，只是操作结果仍是字符串：
```
>>> 'ABCDEFG'[:3]
'ABC'
>>> 'ABCDEFG'[::2]
'ACEG'
```

##迭代
因为Python的for循环不仅可以用在list或tuple上，还可以作用在其他可迭代对象上。

list这种数据类型虽然有下标，但很多其他数据类型是没有下标的，但是，只要是可迭代对象，无论有无下标，都可以迭代，比如dict就可以迭代：
```
>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for key in d:
...     print key
...
a
c
b
```
默认情况下，dict迭代的是key。如果要迭代value，可以用for value in d.itervalues()，如果要同时迭代key和value，可以用for k, v in d.iteritems()。
```Python
>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for value in d.itervalues():
...     print value
... 
1
3
2
>>> for k,v in d.iteritems():
...     print k,v
... 
a 1
c 3
b 2
```
如何判断一个对象是可迭代对象呢？方法是通过collections模块的Iterable类型判断：
```Python
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```
Python内置的enumerate函数可以把一个list变成索引-元素对，这样就可以在for循环中同时迭代索引和元素本身：
```
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print i, value
...
0 A
1 B
2 C
```
`for`循环里，同时引用了两个变量，在Python里是很常见的，比如下面的代码：
```
>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print x, y
...
1 1
2 4
3 9
```

##列表生成式
列表生成式即List Comprehensions，是Python内置的非常简单却强大的可以用来创建list的生成式。
举个例子，要生成list [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]可以用range(1, 11)：
```
>>> range(1, 11)
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
如果要生成[1x1, 2x2, 3x3, ..., 10x10]怎么做？
```Python
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```
for循环后面还可以加上if判断，这样我们就可以筛选出仅偶数的平方：
```Python
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```
还可以使用两层循环，可以生成全排列：
```Python
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```
运用列表生成式，可以写出非常简洁的代码。例如，列出当前目录下的所有文件和目录名，可以通过一行代码实现：
```
>>> import os # 导入os模块
>>> [d for d in os.listdir('.')] # os.listdir可以列出文件和目录
['.emacs.d', '.ssh', '.Trash', 'Adlm', 'Applications', 'Desktop', 'Documents', 'Downloads', 'Library', 'Movies', 'Music', 'Pictures', 'Public', 'VirtualBox VMs', 'Workspace', 'XCode']
```

内建的`isinstance`函数可以判断一个变量是不是字符串：
```
>>> x = 'abc'
>>> y = 123
>>> isinstance(x, str)
True
>>> isinstance(y, str)
False
```

##生成器
列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器（Generator）。
```Python
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x104feab40>
```
如果要一个一个打印出来，可以通过generator的next()方法：
```
>>> g.next()
0
>>> g.next()
1
>>> g.next()
4
>>> g.next()
9
>>> g.next()
16
>>> g.next()
25
>>> g.next()
36
>>> g.next()
49
>>> g.next()
64
>>> g.next()
81
>>> g.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```
generator保存的是算法，每次调用next()，就计算出下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误。

generator非常强大。如果推算的算法比较复杂，用类似列表生成式的for循环无法实现的时候，还可以用函数来实现。

比如，著名的斐波拉契数列（Fibonacci），除第一个和第二个数外，任意一个数都可由前两个数相加得到：

1, 1, 2, 3, 5, 8, 13, 21, 34, ...

斐波拉契数列用列表生成式写不出来，但是，用函数把它打印出来却很容易：
```Python
def fab(max):
    n, a, b = 0, 0, 1
    while n < max:
        print b
        a, b = b, a + b
        n = n + 1
```
上面的函数可以输出斐波那契数列的前N个数：
```
>>> fab(6)
1
1
2
3
5
8
```
仔细观察，可以看出，fab函数实际上是定义了斐波拉契数列的推算规则，可以从第一个元素开始，推算出后续任意的元素，这种逻辑其实非常类似generator。

也就是说，上面的函数和generator仅一步之遥。要把fab函数变成generator，只需要把print b改为yield b就可以了：
```Python
def fab(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
```
这就是定义generator的另一种方法。如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator：
```
>>> fab(6)
<generator object fab at 0x104feaaa0>
```
这里，最难理解的就是`generator`和函数的执行流程不一样。函数是顺序执行，遇到`return`语句或者最后一行函数语句就返回。而变成`generator`的函数，在每次调用`next()`的时候执行，遇到`yield`语句返回，再次执行时从上次返回的yield语句处继续执行。
```Python
>>> def fun():
...     print 'step1'
...     yield
...     print 'step2'
...     yield
...     print 'step3'
...     yield
... 
>>> o = fun()
>>> o.next()
step1
>>> o.next()
step2
>>> o.next()
step3
>>> o.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```
