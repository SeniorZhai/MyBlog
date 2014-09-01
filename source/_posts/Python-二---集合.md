title: Python 二 - 集合
date: 2014-06-12 13:13:44
categories: Python
tags: [Python]
---
##list 和 tuple

###list
list是Python内置的一种数据类型是列表，为一种有序的集合，可以随时添加和删除其中的元素。
例如，列出班级中所有同学的名字：
```Python
classmates = ['Michael','Bob','Tracy']
print classmates
```
可以用`len()`函数获取list元素的个数
```
>>> len(classmates)
3
```
用索引来访问list中每一个位置的元素，记得索引是从0开始的：
```Python
>>> classmates[0]
'Michael'
>>> classmates[1]
'Bob'
>>> classmates[2]
'Tracy'
>>> classmates[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```
当索引超出了范围时，Python会报一个IndexError错误，所以，要确保索引不要越界，记得最后一个元素的索引是`len(classmates) - 1`。

如果要取最后一个元素，除了计算索引位置外，还可以用-1做索引，直接获取最后一个元素：
```
>>> classmates[-1]
'Tracy'
```
以此类推，可以获取倒数第2个、倒数第3个：
```python
>>> classmates[-2]
'Bob'
>>> classmates[-3]
'Michael'
>>> classmates[-4]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```
当然，倒数第4个就越界了。

list是一个可变的有序表，所以，可以往list中追加元素到末尾：
```Python
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
```
也可以把元素插入到指定的位置，比如索引号为1的位置：
```Python
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```
要删除list末尾的元素，用pop()方法：
```Python
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']
```
要删除指定位置的元素，用pop(i)方法，其中i是索引位置：
```Python
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
```
要把某个元素替换成别的元素，可以直接赋值给对应的索引位置：
```Python
>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
```
list里面的元素的数据类型也可以不同，比如：
```Python
>>> L = ['Apple', 123, True]
```
list元素也可以是另一个list，比如：
```Python
>>> s = ['python', 'java', ['asp', 'php'], 'scheme']
>>> len(s)
4
```
要注意s只有4个元素，其中s[2]又是一个list，如果拆开写就更容易理解了：
```Python
>>> p = ['asp', 'php']
>>> s = ['python', 'java', p, 'scheme']
```
要拿到'php'可以写p[1]或者s[2][1]，因此s可以看成是一个二维数组，类似的还有三维、四维……数组，不过很少用到。

如果一个list中一个元素也没有，就是一个空的list，它的长度为0：
```Python
>>> L = []
>>> len(L)
0
```

##tuple
另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改，比如同样是列出同学的名字：
```Python
>>> classmates = ('Michael', 'Bob', 'Tracy')
```
现在，classmates这个tuple不能变了，它也没有append()，insert()这样的方法。其他获取元素的方法和list是一样的，你可以正常地使用classmates[0]，classmates[-1]，但不能赋值成另外的元素。

不可变的tuple有什么意义？因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple。

tuple的陷阱：当你定义一个tuple时，在定义的时候，tuple的元素就必须被确定下来，比如：
```Python
>>> t = (1, 2)
>>> t
(1, 2)
```
如果要定义一个空的tuple，可以写成()：
```Python
>>> t = ()
>>> t
()
```
但是，要定义一个只有1个元素的tuple，如果你这么定义：
```Python
>>> t = (1)
>>> t
1
```
定义的不是tuple，是1这个数！这是因为括号()既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是1。
所以，只有1个元素的tuple定义时必须加一个逗号,，来消除歧义：
```Python
>>> t = (1,)
>>> t
(1,)
```
Python在显示只有1个元素的tuple时，也会加一个逗号,，以免你误解成数学计算意义上的括号。

最后来看一个“可变的”tuple：
```Python
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```
这个tuple定义的时候有3个元素，分别是'a'，'b'和一个list。不是说tuple一旦定义后就不可变了吗？怎么后来又变了？

别急，我们先看看定义的时候tuple包含的3个元素：
![](https://github.com/zt1991616/blog/raw/Image/14031501.png)
当我们把list的元素'A'和'B'修改为'X'和'Y'后，tuple变为：
![](https://github.com/zt1991616/blog/raw/Image/14031502.png)
表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。tuple一开始指向的list并没有改成别的list，所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向'a'，就不能改成指向'b'，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！

###小结
list和tuple是Python内置的有序集合，一个可变，一个不可变。根据需要来选择使用它们。

##条件判断和循环
###条件判断
根据Python的缩进规则，如果`if`语句判断是`True`,就把缩进的语句执行，否则不执行。
也可以给`if`添加一个`else`语句，如果`if`判断是`False`，就不执行`if`的内容，而执行`else`的内容
```Python
if(3>2):
	print '执行这里'
else:
	print '没执行这里'
```
注意不要少写`冒号":"`
```Python
elif是else if的缩写，完全可以有多个elif，所以if语句的完整形式就是：
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
```
###循环
Python的循环有两种，一种是for...in循环，用于遍历list或者tuple
```Python
colors = ['red','blue','green']
for color in colors:
	print color
```
执行这段代码会依次打印`colors`的每一个元素
```
red
blue
green
```
Python提供了一个range()函数，可以生成一个整数序列，我们可以计算一个1-100的整数之和
```Python 
sum = 0
for x in range(101):
	sum = sum + x
print sum
```
第二种循环是`while`循环，和C语言一样，只要条件为`True`，就会执行循环体
```Python
sum = 0
n = 1
while n <= 100:
	sum = sum + n
	n = n + 1
print sum
```
如果死循环了，记得用`Ctrl + C`退出循环

##dict和set

###dict
dict就是dictionary，在Java里也成为map，使用键值对（key-value）存储，具有极快的查找速度
例如记录同学们的成绩
```Python
d = {'Michael':95,'Bob':75,'Tracy':85}
print d['Micheal']
```
除了初始化时指定外，存储value时，通过key放入(相同的key赋值的话，会覆盖掉之前的value)：
```Python
>>> d['Adam'] = 76
```
如果key不存在dict会报错，为了避免这种错误，可以通过`in`判断key是否存在，或者使用dict的`get`方法，获取value，不存在的话会返回None
```
dic = {"a":67,"c":12,"b":1}
print 'd' in dic
print dic.get("d")
```
要删除一个key，使用`pop(key)`方法
与list相比，dict有以下几个特点
1. 查找和插入的速度极快，不会随着key的增加而增加
2. 需要占用大量内存，内存浪费多
而list相反
1. 查找和插入的时间随着元素的增加而增加
2. 占用空间小，浪费内存很小

###Set
Set和dict类似，但是不存在value，只存一组key,而且key不能重复，所以Set中没有重复的key
```Python
>>> s = set([1,2,3,4,5,1,2,3,4,5])
>>> s
set([1, 2, 3, 4, 5])
```
重复的元素会被自动过滤掉
添加元素可以使用set的`add(key)`方法，可以重复添加，但不会有效果，删除元素使用`remove(key)`方法
```Python
>>> s = set([])
>>> s.add(1)
>>> s.add(2)
>>> s.add(3)
>>> s.add(4)
>>> s.remove(3)
>>> s
set([1, 2, 4])
```
注意删除的元素必须存在，否则报错
set可以看成数学上的无序和无重复元素的集合，所以也可以做交集和并集的运算操作
```Python
>>> s1 = set([1,2,3,4])
>>> s2 = set([2,4,6,8])
>>> s1 & s2
set([2, 4])
>>> s1 | s2
set([1, 2, 3, 4, 6, 8])
```

##函数
Python内置了很多很有用的函数，我们可以直接调用。可以在Python的官网上查看[文档](http://docs.python.org/2/library/functions.html)
可以在命令行通过`help()`查看某个函数的帮助信息
```Python
help(abs)
Help on built-in function abs in module __builtin__:

abs(...)
    abs(number) -> number
    
    Return the absolute value of the argument.
```

###数据类型转换
Python内置常用的函数，比如int()函数就可以把其他数据类型转换为整数：
```Python
>>> int('123')
123
```
函数名其实就是函数对象的引用，所以可以用一个变量指向该引用，实现别名的效果
```Python
>>> a = abs
>>> a(-1)
1
```
主要使用函数时，一定要传入正确的参数，否则会出错。

###函数的定义
在Python中，定义一个函数要使用`def`语句，依次写出函数名、括号、括号中的参数和冒号:，然后，在缩进块中编写函数体，函数的返回值用`return`语句返回。
```Python
def fun(x,y):
  return x+y
```
###空函数
定义一个什么事也不做的空函数，可以使用`pass`语句
```Python
def nop():
  pass
```
###参数检查
Python会进行参数个数的检查，如果不对会抛出`TypeError`，但是类型并不会进行检查

###默认参数
在定义时为某个参数初始化一个值，若调用时，该参数缺省时，便会使用默认的值
```
def power(x,n = 2):
  s = 1
  while n > 0:
    n = n - 1
    s = s * x
  return s
```
###可变参数
函数参数个数可变时，我们可以通过list或者tuple包装参数，传入函数，函数会接收到tuple
```
def add(numbers):
  sum = 0
  for number in numbers:
    sum = sum + number
  return sum

add([1,2,3,4,5])
```
```
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```
如果已经有一个list或者tuple，要调用一个可变参数怎么办？可以这样做：
```
>>> nums = [1, 2, 3]
>>> calc(nums[0], nums[1], nums[2])
14
```

###关键字参数
关键字参数允许传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装成为一个dict
```Python
def person(name,age,**kw):
  print 'name:',name,'age':,age,'other:',kw
```
函数`person`除了参数`name`和`age`外，还接受关键字参数`kw`。在调用该函数时，可以之传入必选参数，或者任意个关键字参数
```
>>> person('Michael', 30)
name: Michael age: 30 other: {}
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

###参数组合
在Python中定义函数，可以用必选参数、默认参数、可变参数和关键字参数，这4种参数都可以一起使用，或者只用其中的一部分，但定义的顺序必须是：必选参数、默认参数、可变参数和关键字参数。
比如：
```Python
def fun(a,b,c=0,*args,**kw)
  print 'a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw
```
调用时，Python接收器自动按照参数位置和参数名把对应的参数传进去
```
>>> func(1, 2)
a = 1 b = 2 c = 0 args = () kw = {}
>>> func(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}
>>> func(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
>>> func(1, 2, 3, 'a', 'b', x=99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
```
最神奇的是通过一个tuple和dict，你也可以调用该函数：
```
>>> args = (1, 2, 3, 4)
>>> kw = {'x': 99}
>>> func(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'x': 99}
```

###递归函数
如果一个函数在内部调用自身，这个函数就是递归函数。
在Python中使用递归函数要注意`栈溢出`,在计算机中，函数调用是通过栈（stack）这种数据结构实现的，每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致栈溢出。
解决递归调用栈溢出的方法是通过尾递归优化，事实上尾递归和循环的效果是一样的，所以，把循环看成是一种特殊的尾递归函数也是可以的。
- 尾递归是指，在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。
