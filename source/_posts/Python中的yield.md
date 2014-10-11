title: Python中的yield
date: 2014-10-10 14:33:42
categories: Python
tags: [yield,Python]
---
<!--more-->
如果函数中包含`yield`关键字，那么这个函数就不再是一个普通函数，而是一个generator。而变成generator的函数在每次调用`next()`的时候执行遇到`yield`语句返回，再次执行时从上一次返回的`yield`语句继续执行
```python
def fib(max):
	a,b,c = 0,1,0
	while c < max:
		yield b
		a,b,c = b,a+b,c+1
```

	>>> o = fib(1000)
	>>> o.next()
	1
	>>> o.next()
	1
	>>> o.next()
	2
	>>> o.next()
	3
	>>> o.next()