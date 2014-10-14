title: Lambda
date: 2014-10-13 11:17:22
categories: Python
tags: [Python]
---
lambda函数也叫匿名函数即函数没有具体的名称
lambda语句用来创建新的函数对象，并且在运行时返回它们
<!--more-->
```python
def make_repeater(n):
	return lambda s: s * n

twice = make_repeater(2)

print twice('word') # wordword
print twice(5) # 10
```
lambda语句用来创建函数对象，本质上，lambda需要一个参数，后面仅跟单个表达式作为函数体，而表达式的值被这个新建的函数返回。
主要的作用：
1. 使用Python写一些脚本时，使用lambda可以省去定义函数的过程
2. 对于一些抽象，不会别的地方再复用的函数可以使用
3. 使用lambda在某些时候可以让代码更容易理解
