title: Python 四 - 函数式编程
date: 2014-06-14 11:15:54
categories: Python
tags: [Python]
---
函数是一种把大段代码封装的一种形式，通过一层层的函数调用，可以把复杂的任务分解成简单的任务，这种分解可以称之为面向过程的程序设计。函数就是面向过程的程序设计的基本单元。
函数式编程——Functional Programming可以归结到面向过程的程序设计，但其思想更接近数学计算。
函数式编程就是一种抽象程度很高的编程范式，纯粹的函数式编程编写的函数没有变量。因此任意一个函数只要输入是确定的，输出就是确定的，这种函数我们称之为没有副作用。而允许使用变量的程序设计语言，由于韩式内部的变量状态不稳定，同样的输入，可能得到不同的输出，因此这种函数就是有副作用的。
函数式编程的一个特点就是允许把函数本身作为参数传入另一个函数，还允许返回一个函数！
Python对函数式编程提供部分支持，由于Python允许使用变量，因此，Python不是纯函数式编程语言。
## 高阶函数
### 传入函数
`map()`函数接收两个参数，一个函数，一个是序列，`map`将传入函数一次作用到序列的每个元素，并把结果作为新的list返回。
举例说明，比如我们有一个函数f(x)=x^2，要把这个函数作用在一个list `[1, 2, 3, 4, 5, 6, 7, 8, 9]`上，就可以用map()实现如下：
![](https://github.com/zt1991616/blog/raw/Image/14031401.png)
```
>>> def f(x):
...     return x * x
...
>>> map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```
像`map()`函数这种能够接收函数作为参数的函数，称之为`高阶函数`（`Higher-order function`）。

再看reduce的用法。reduce把一个函数作用在一个序列[x1, x2, x3...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算，其效果就是：
```
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```
比方说对一个序列求和，就可以用reduce实现：
```Python
>>> def add(x, y):
...     return x + y
...
>>> reduce(add, [1, 3, 5, 7, 9])
25
```
考虑到字符串str也是一个序列，对上面的例子稍加改动，配合map()，我们就可以写出把str转换为int的函数：
```Python
>>> def fn(x, y):
...     return x * 10 + y
...
>>> reduce(fn, map(int, '13579'))
13579
```
整理成一个str2int的函数就是：
```Python
def str2int(s):
    def fn(x, y):
        return x * 10 + y
    return reduce(fn, map(int, s))
```
还可以用lambda函数进一步简化成：
```Python
def str2int(s):
    return reduce(lambda x,y: x*10+y, map(int, s))
```

### 排序算法
Python内置的`sorted()`函数可以对list进行排序
```Python
>>> sorted([34,12,43,65,1])
[1, 12, 34, 43, 65]
```
`sorted()`函数也是一个高阶函数，它还可以接收一个比较函数来实现自定义的排序，比如实现一个倒叙排序
```Python
def reversed_cmp(x,y):
	if x > y:
		return -1
	if x < y:
		return 1
	return 0
```
传入自定义的比较函数reversed_cmp，就可以实现倒序排序：
```Python
>>> sorted([36, 5, 12, 9, 21], reversed_cmp)
[36, 21, 12, 9, 5]
```
再看一个字符串排序的例子：
```Python
>>> sorted(['about', 'bob', 'Zoo', 'Credit'])
['Credit', 'Zoo', 'about', 'bob']
```
默认情况下，对字符串排序，是按照ASCII的大小比较的，由于'Z' < 'a'，结果，大写字母Z会排在小写字母a的前面。

### 函数作为返回值
高阶函数除了可以接收函数作为参数外，也可以把函数作为结果值返回。
```Python
def lazy_sum(*args):
	def sum():
		ax = 0
		for n in args:
			ax = ax + n
		return ax
	return sum
```
调用lazy_sum()时，返回的并不是求和结果，而是求和函数：
```
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function sum at 0x10452f668>
>>> f()	# 调用函数f时，才真正计算求和的结果
```

### 匿名函数
当我们在传入函数时，有时候，不需要显式地定义函数，直接传入匿名函数更方便。
在Python中，对匿名函数提供了有限支持。还是以map()函数为例，计算f(x)=x2时，除了定义一个f(x)的函数外，还可以直接传入匿名函数：
```Python
>>> map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9])
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```
通过对比可以看出，匿名函数lambda x: x * x实际上就是：
```
def f(x):
    return x * x
```
关键字`lambda`表示匿名函数，冒号前面的`x`表示函数参数。

匿名函数有个限制，就是只能有一个表达式，不用写`return`，返回值就是该表达式的结果。

用匿名函数有个好处，因为函数没有名字，不必担心函数名冲突。此外，匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数：
```Python
>>> f = lambda x: x * x
>>> f
<function <lambda> at 0x10453d7d0>
>>> f(5)
25
```
同样，也可以把匿名函数作为返回值返回，比如：
```Python
def build(x, y):
    return lambda: x * x + y * y
```

### 装饰器
由于函数也是一个对象，而且函数对象可以被赋值给变量，所以通过变量也能调用该函数
```Python
>>> def now():
...     print '2014-03-16'
... 
>>> f = now
>>> f()
2014-03-16
```
函数对象有一个`__name__`属性，可以拿到函数的名字：
```Python
>>> now.__name__
'now'
>>> f.__name__
'now'
```
假设我们要增强now()函数的功能，比如，在函数调用前后自动打印日志，但又不希望修改now()函数的定义，这种在代码运行期间动态增加功能的方式，称之为“`装饰器`”（`Decorator`）。
本质上，decorator就是一个返回函数的高阶函数。所以，我们要定义一个能打印日志的decorator，可以定义如下：
```
def log(func):
    def wrapper(*args, **kw):
        print 'call %s():' % func.__name__
        return func(*args, **kw)
    return wrapper
```
观察上面的`log`，因为它是一个decorator，所以接受一个函数作为参数，并返回一个函数。我们要借助Python的@语法，把decorator置于函数的定义处：
```Python
@log
def now():
    print '2013-12-25'
```
调用`now()`函数，不仅会运行`now()`函数本身，还会在运行`now()`函数前打印一行日志：
```Python
>>> now()
call now():
2013-12-25
```
把@log放到`now()`函数的定义处，相当于执行了语句：
```Python
now = log(now)
```
由于`log()`是一个`decorator`，返回一个函数，所以，原来的`now()`函数仍然存在，只是现在同名的`now`变量指向了新的函数，于是调用`now()`将执行新函数，即在`log()`函数中返回的`wrapper()`函数。
`wrapper()`函数的参数定义是(*args, **kw)，因此，`wrapper()`函数可以接受任意参数的调用。在wrapper()函数内，首先打印日志，再紧接着调用原始函数。

如果decorator本身需要传入参数，那就需要编写一个返回decorator的高阶函数，写出来会更复杂。比如，要自定义log的文本：
```Python
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print '%s %s():' % (text, func.__name__)
            return func(*args, **kw)
        return wrapper
    return decorator
```
这个3层嵌套的decorator用法如下：
```Python
@log('execute')
def now():
    print '2013-12-25'
```
执行结果如下：
```Python
>>> now()
execute now():
2013-12-25
```
和两层嵌套的decorator相比，3层嵌套的效果是这样的：
```Python
>>> now = log('execute')(now)
```
我们来剖析上面的语句，首先执行log('execute')，返回的是decorator函数，再调用返回的函数，参数是now函数，返回值最终是wrapper函数。

以上两种decorator的定义都没有问题，但还差最后一步。因为我们讲了函数也是对象，它有`__name__`等属性，但你去看经过decorator装饰之后的函数，它们的`__name__`已经从原来的'now'变成了'wrapper'：
```Python
>>> now.__name__
'wrapper'
```
因为返回的那个`wrapper()`函数名字就是'wrapper'，所以，需要把原始函数的`__name__`等属性复制到wrapper()函数中，否则，有些依赖函数签名的代码执行就会出错。

不需要编写`wrapper.__name__ = func.__name__`这样的代码，Python内置的`functools.wraps`就是干这个事的，所以，一个完整的decorator的写法如下：
```Python
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print 'call %s():' % func.__name__
        return func(*args, **kw)
    return wrapper
```
或者针对带参数的decorator：
```Python
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print '%s %s():' % (text, func.__name__)
            return func(*args, **kw)
        return wrapper
    return decorator
```
`import functools`是导入`functools`模块。模块的概念稍候讲解。现在，只需记住在定义`wrapper()`的前面加上`@functools.wraps(func)`即可。

### 偏函数
Python的`functools`模块提供了很多有用的功能，其中一个就是偏函数(Parial function)
`functools.partial`的作用就是，把一个函数的某些参数（不管有没有默认值）给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。
```Python
>>> import functools
>>> def fun(i):
...     print i
... 
>>> fun2 = functools.partial(fun,i = 1)
>>> fun2()
1
```