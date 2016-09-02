title: Python 六 - 面向对象编程
date: 2014-06-18 14:12:31
categories: Python
tags: [Python]
---
面向对象编程————Object Oriented Programming，简称OOP，是一种程序设计思想。OOP吧对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。

面向过程的程序设计把计算机程序视为一系列的命令集合，即一组函数的顺序执行，为了简化程序设计，面向过程吧函数继续切分成为子函数，即把大块的函数通过切割成小块函数降低系统的复杂度。

而面向对象的程序设计吧计算机程序视为一组对象的集合，而每个对象都可以接受其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。

在Python中，所有数据类型都可以视为对象，当然也可以自定义对象，自定义的对象数据类型就是面向对象中的类(Class)的概念。

## 类和实例
面向对象最重要的概念就是类（Class）和实例（Instance），必须牢记类是抽象的模板，而实例是根据类创建出来的一个个具体的‘对象’，每个对象都拥有相同的方法，但各自的数据可能不同。
以Student类为例，在Python中，定义类是通过`class`关键字
```Python
class Student(object):
	pass
```
`clas`后米娜紧接着是类名，即'Student'，类名通过是大写开头的单词，紧接着是`(object)`，表示类是从哪个类继承下来的，`object`是所有类都会继承的类。
定义了好了`Student`类，就可以根据`Student`类创建出`Student`的实例，创建实例是通过类名+()实现的：
```Python
>>> jack = Student()
>>> jack
<__main__.Student object at 0x10f5e3110>
>>> Student
<class '__main__.Student'>
>>> 
通过一个特殊的`__init__`方法，在创建实例的时候，就可以对某些属性初始化
```Python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score
```
注意__innit__方法的第一个参数永远是`self`，表示创建的实例本身，因此在`__init__`方法内部，就可以把各个属性绑定到`self`，因为`self`就指向创建的实例本身。

有了`__init__`方法，在创建实例的时候，就不能传入空的参数了，必须传入与`__init__`方法匹配的参数，但`self`不需要传，Python解释器自己会把实例变量穿进去：
```Python
>>> class Student(object):
...     
...     def __init__(self,name):
...             self.name = name
... 
>>> jack = Student('jack')
```

## 数据封装
面向对象编程的一个重要特点就是数据封装。
类本身就拥有数据，要访问数据，就没有必要从外面的函数去访问，就直接用类内部定义访问数据的函数，这样就把“数据”给封装起来了，这些封装数据的函数是和类本身关联起来的，外面称之为类的方法，要定义一个方法，除了第一个参数是`self`外，其他和普通函数一样。要调用一个方法，只需要在实例变量上直接调用，出了`self`不用传递，其他参数正常传入。
```Python
>>> class Student(object):
...     def __init__(self,name):
...             self.name = name
...     def getName(self):
...             return self.name
... 
>>> jack = Student('jack')
>>> jack.getName()
'jack'
```

## 访问限制
在Class内部，可以有属性和方法，而外部代码可以通过直接调用实例变量的方法来操作数据，这样就可以隐藏内部的复杂逻辑。
为了内部属性不被外部访问，可以把属性的名称前加上两个下划线`__`，在Python中，实例变量名如果以`__`开头，就变成了一个私有变量(private)，只有内部可以访问，外部不能访问。
需要注意的是，在Python中变量名类似`__xxx__`的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量，所以不能用__name__这样的变量名。
双下划线开头的实例变量外部不能直接访问是因为Python解释器对外把`__name`变量改成了`_Student__name`，所以，仍然可以通过`_Student__name`来访问`__name`变量。
```Python
#  /usr/bin/env python
#  -*- coding:utf-8 -*-

class Student(object):
	def __init__(self,name):
		self.__name = name
	def print_name(self):
		print '%s' % (self.__name)

jack = Student('jack')
jack.print_name()
print jack._Student__name
# 执行结果
# jack
# jack
```
但不同版本的Python解释器可能会把`__name`改成不同的变量名。

## 继承和多态
在OOP程序设计中，定义一个class的时候，可以从某个现有的class继承，新的class称为子类(Subclass)，而被继承的class称为基类、父类或者超类(Base class、Super class)
```Python
class Animal(object):
	def run(self):
		print 'Animal is running...'

class Dog(Animal):
	def run(self):
		print 'Dog is running...'

class Cat(Animal):
	pass

dog = Dog()
dog.run()

cat = Cat()
cat.run()
# 运行结果
# Dog is running...
# Animal is running...
```
判断一个变量是否是某个类型可以用`isinstance()`判断：
```Python
>>> a = list() #  a是list类型
>>> b = Animal() #  b是Animal类型
>>> c = Dog() #  c是Dog类型
>>> isinstance(a,list)
True
>>> isinstance(b,list)
False
>>> isinstance(b,Animal)
True
>>> isinstance(c,Animal)
True
>>> isinstance(c,Dog)
True
```

## 获取对象信息

### 使用type()
判断对象类型，使用`type()`函数
```python
>>> type(123)
<type 'int'>
>>> type('str')
<type 'str'>
>>> type(None)
<type 'NoneType'>
>>> type(abs)
<type 'builtin_function_or_method'>
```
Python把每种type类型都定义好了常量，放在types模块里，使用之前，需要先导入：
```Python
>>> import types
>>> type('abc')==types.StringType
True
>>> type(u'abc')==types.UnicodeType
True
>>> type([])==types.ListType
True
>>> type(str)==types.TypeType
True
```

### 使用isinstance()
对于class的继承关系，可以用`isinstance()`函数判断
```Python
>>> class Animal(object):
...     pass
... 
>>> class Dog(Animal):
...     pass
... 
>>> class Husky(Dog):
...     pass
... 
>>> a = Animal()
>>> d = Dog()
>>> h = Husky()
>>> isinstance(h,Husky)
True
>>> isinstance(h,Dog)
True
>>> isinstance(h,Animal)
True
>>> isinstance(d,Husky)
False
>>> isinstance(d,Dog)
True
>>> isinstance(d,Animal)
True

>>> isinstance(d,(Animal,Husky))#  d是否为Animal,Husky中的一种
True
```

### 使用dir()
获取一个对象的所有属性和方法，可以使用`dir()`函数，它返回一个包含字符串的list
```Python
>>> dir('ABC')
['__add__', '__class__', '__contains__', '__delattr__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__getslice__', '__gt__', '__hash__', '__init__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '_formatter_field_name_split', '_formatter_parser', 'capitalize', 'center', 'count', 'decode', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'index', 'isalnum', 'isalpha', 'isdigit', 'islower', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```
类似`__xxx__`的属性和方法在Python中都有特殊的用途，比如上例的`__len__`方法返回长度。调用`len()`函数获取对象长度时，实际回去调用对象的`__len__`方法。
利用`getattr()`、`setattr()`、`hasattr()`函数还可以直接操作一个对象的状态：
```Python
>>> obj = MyObject()
>>> hasattr(obj,'x')#  是否有属性'x'？
True
>>> hasattr(obj,'y')#  是否有属性'y'？
False
>>> setattr(obj,'y',19)#  设置一个属性'y'
>>> hasattr(obj,'y')#  是否有属性'y'？
True
>>> getattr(obj,'y')#  或属性属性'y'
19
```
可以传入一个default参数，如果属性不存在，就返回默认值：
```Python
>>> getattr(obj, 'z', 404) #  获取属性'z'，如果不存在，返回默认值404
404
```
## 动态绑定属性和方法
正常情况下，当我们定义了一个class，差创建一个class的实例后，我们可以给该实例绑定任何属性和方法，这就是动态语言的灵活性：
```Python
>>> class Student(object):
...     pass
... 
>>> s = Student()
>>> s.name = 'Zoe'
>>> print s.name
Zoe
>>> def set_age(self,age): # 定义一个函数作为实例的方法
...     self.age = age
... 
>>> from types import MethodType
>>> s.set_age = MethodType(set_age,s,Student) #  给实例绑定一个方法
>>> s.set_age(22)
>>> s.age
22
```
但是其他的实例该方法是不起作用的
```Python
>>> s1 = Student()
>>> s2.set_age(21)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 's2' is not defined
```
可以给class绑定方法：
>>> Student.set_age = MethodType(set_age,None,Student)
>>> s1.set_age(21)
>>> s1.age
21
>>> s.age
22
```

## 使用__slots__
如果想要限制class的属性怎么办？比如只允许Student实例添加`name`和`age`属性。
为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的`__slots__`变量，来限制该class能添加的属性：
```Python
>>> s = Student()
>>> s.name = 'Zoe'
>>> s.age = 22
>>> s.score = 99
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```
注意，`__slots__`定义的属性仅对当前类起作用，对继承的子类是不起作用的

## @property
在绑定属性时，我们直接把属性暴露出来，这样还不安全
为了限制属性的使用范围，可以使用`@property`装饰器把一个方法变成属性调用：
```Python
>>> class Student(object):
...     @property
...     def score(self):
...             return self._score
...     @score.setter
...     def score(self,value):
...             if not isinstance(value,int):
...                     raise ValueError('score must be an integer!')
...             if value < 0 or value > 100:
...                     raise ValueError('score must between 0 ~ 100')
...             self._score = value
```
@property的实现比较复杂，我们先考察如何使用。把一个getter方法变成属性，只需要加上@property就可以了，此时，@property本身又创建了另一个装饰器@score.setter，负责把一个setter方法变成属性赋值，于是，我们就拥有一个可控的属性操作：
```
>>> s = Student()
>>> s.score = 69
>>> s.score 
69
>>> s.score = 101
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 10, in score
ValueError: score must between 0 ~ 100
>>> s.score = '99'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 8, in score
ValueError: score must be an integer!
```
还可以定义只读属性，只定义getter方法，不定义setter方法就是一个只读属性：
```
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2014 - self._birth
```
上面的birth是可读写属性，而age就是一个只读属性，因为age可以根据birth和当前时间计算出来。


## 多重继承
Python支持同时继承多个父类
```Python
class SubClass(SpuClass1,SpuClass2,...)
```

## Mixin
在设计类的继承关系时，通常主线是单一继承下来的。为了混入额外的功能，可以通过多继承实现，这种设计称为Mixing。
为了更好地看出继承关系，可以在添加功能的类后面在Mixin，比如`class Dog(Mammal,RunnableMixin,CarnivorousMixin)`
Python子弟啊的很多库使用了Mixi,举个例子，Python自带的`TCPServer`和`UDPServer`这两个网络服务，而要同时多个用户就必须使用多进程或多线程模型，这两种模型由`ForkingMixin`和`ThreadingMixin`提供。

## 定制类
之前，我们知道了一些形如`__xxx__`的变量或方法的特殊作用，如：`__slots__`，`__len__()`

### __Str__
类的说明
```Python
ValueError: score must be an integer!
>>> class Student(object):
...     def __init__(self,name):
...             self.name = name
... 
>>> print Student('Jack')
<__main__.Student object at 0x10e50d910>
>>> class Student(object):
...     def __init__(self,name):
...             self.name = name
...     def __str__(self):
...             return 'Student object (name:%s)' % self.name
... 
>>> print Student('Jack')
Student object (name:Jack)
>>> s
<__main__.Student object at 0x10e50d790>	#  直接输出还是“不好看”
>>> class Student(object):
...     def __str__(self):
...             return 'Student object (name:%s)' % self.name
...     def __init__(self,name):
...             self.name = name
...     __repr__ = __str__
... 
>>> s = Student("Jakc")
>>> s
Student object (name:Jakc)

```

### __iter___
如果一个类要呗用于`for...in`循环，就必须实现一个`__iter__()`方法，该方法返回一个迭代对象，然后Python的for循环就会不断调用该迭代对象的`next()`方法拿到循环的下一个值，值得遇到StopIteration错误时退出循环。
```Python
>>> class Fib(object):
...     def __init__(self):
...             self.a,self.b = 0,1
...     def __iter__(self):
...             return self
...     def next(self):
...             self.a,self.b = self.b,self.a + self.b
...             if self.a > 100000:
...                     raise StopIteration()
...             return self.a
... 
>>> for n in Fib():
...     print n
... 
1
1
2
3
5
8
13
21
34
55
89
144
233
377
610
987
1597
2584
4181
6765
10946
17711
28657
46368
75025
```

### __getitem__
要想像list那样按照下标取元素，需要实现__getitem__()方法
```Python
>>> class Fib(object):
...     def __getitem__(self,n):
...             a,b = 1,1
...             for x in range(n):
...                     a,b = b,a+b
...             return a
... 
>>> f = Fib()
>>> f[0]
1
>>> f[1]
1
>>> f[10]
89
>>> f[20]
10946
>>> f[30]
1346269
```
但是list有个神奇的切片方法：
```Python
>>> range(100)[5:10]
[5, 6, 7, 8, 9]
```
对于Fib却报错。原因是__getitem__()传入的参数可能是一个int，也可能是一个切片对象slice，所以要做判断：
```Python
class Fib(object):
    def __getitem__(self, n):
        if isinstance(n, int):
            a, b = 1, 1
            for x in range(n):
                a, b = b, a + b
            return a
        if isinstance(n, slice):
            start = n.start
            stop = n.stop
            a, b = 1, 1
            L = []
            for x in range(stop + 1):
                if x >= start:
                    L.append(a)
                a, b = b, a + b
            return L
```

### __getattr__
通过`__getattr__()`方法我们可以返回一个不存在的属性
```Python
>>> class SubClass(object):
...     def __getattr__(self,name):
...             if name=='age':
...                     return lambda:22
... 
>>> s = SubClass()
>>> s.age()
22
```

### __call__
设置一个函数，通过实例本身调用
```Python
>>> class Student(object):
...     def __init__(self,name):
...             self.name = name
...     def __call__(self):
...             print('My name is %s' % self.name)
... 
>>> s = Student('zoe')
>>> s()
My name is zoe
```
通过callable()函数，我们就可以判断一个对象是否是“可调用”对象。
```Python
>>> callable(max)
True
>>> callable([1,2,4])
False
>>> callable(None)
False
>>> callable('str')
False
>>> callable(Student('zoe'))
True
```

