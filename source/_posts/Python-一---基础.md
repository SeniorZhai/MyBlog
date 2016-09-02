title: Python 一 - 基础
date: 2014-06-12 11:12:36
categories: Python
tags: [Python]
---

断断续续的学了几次Python了，所谓不求甚解，语法过了一遍没去用忘了，又过一遍又没去用又忘了。虽然现在用Python的机会不多，还是真心喜欢这门语言。所以还是另起门户，把学习笔记记下来，免得自己捡起来又扔掉了。

Python是著名的“龟叔”Guido van Russum在1989年圣诞节期间，为了打发无聊的圣诞节而编写的一个编程语言（我的天。。。）。
牛逼闪闪的“龟叔”
![](https://github.com/zt1991616/blog/raw/master/Image/14031401.jpg)
Python提供了非常完善的基础代码库，覆盖了网络、文件、GUI、数据库、文本等大量内容，还有大量的第三方库。

Python的定位是`优雅`、`明确`、`简单`，Python是不能加密的，发布Python程序就相当于发布了源代码（到处都能看源代码，想想都有点小激动呢）。

## 安装Python

### Mac
Python是Mac上的一等公民，天生自带。

### Windows

1.从[Python官网](www.python.org)在下2.7.X版本
2.运行`.MSI`安装包，一路`Nest`即可
3.添加环境变量，即把Python的安装路径添加到系统环境变量`path`中去

### Linux

没实践过不表，相信google、百度一大把

## First Blood

国际惯例，我们编写一个`Hello,World`

在命令行里计入Python交互环境（Python，回车）：
```
>>> print "Hello,world"
Hello,world
```
完成。

## 输入与输出

### 输出
用`print`加上字符串，就可以向屏幕上输出指定的文字。比如输出`'hello, world'`，用代码实现如下：
```
>>> print 'hello, world'
```
print语句也可以跟上多个字符串，用逗号“,”隔开，就可以连成一串输出：
```
>>> print 'The quick brown fox', 'jumps over', 'the lazy dog'
The quick brown fox jumps over the lazy dog
```

### 输入
Python提供了一个raw_input，可以让用户输入字符串，并存放到一个变量里。比如输入用户的名字：
```
>>> name = raw_input()
zoe
>>> name
'zoe'
```
当你输入`name = raw_input()`并按下回车后，Python交互式命令行就在等待你的输入了。这时，你可以输入任意字符，然后按回车后完成输入。
我们在文本编辑器中编写一个完整版的试试：
```
# !/usr/bin/env python

name = raw_input('please enter your name:')
print 'hello,',name

```
在终端运行，切换到文件的目录(注意权限，可能需要`chmod a+x`)
```
./hello.py
please enter your name:zoe
hello, zoe
```

## Python基础

Python以`# `开头的语句是注解，注解是给人看的，所以解释器会忽略掉注释。其他没一行都是一个语句，当语句以冒号`":"`结尾时，所接的语句视为代码块。
还有一点值得注意，Python是`大小写敏感`的，大小写区分，如果写错了大小写，程序会报错

### 数据类型

#### 整数
Python可以处理任意大小的整数，包括负整数，如：`1`,`1000`,`-1000`,`0`等，十六进度也使用`0x`前缀和`0-9`、`a-f`表示，如：`0xffff`,`0xabcd1234`等

#### 浮点数
数学写法，如`1.23`、`-4.5`、`3.1415926`，科学计数法，把10用e表示，即`1.23e9`或`13.54e-6`。

#### 字符串
字符串是以''或""括起来的任意文本，比如"abc",'123xyz'.Python也有转义字符，如`\n`、`\t`、`\'`等等。
Python 还允许用`r''`表示''内的字符串默认不转义：
```
>>> print '\\\t\\'
\	\
>>> print r'\\\t\\'
\\\t\\
```
Python允许使用`'''...'''`表示多行内容
```
>>> print '''line1
... line2
... line3
... line4'''
line1
line2
line3
line4
```

#### 布尔值

布尔只存在`True`、`False`两种值（注意大小写）
通过布尔运算：
```
>>> 3 > 3
False
>>> 3 >= 3
True
>>> 3 > 2
True
```

布尔值可以用and、or和not运算。

and运算是`与运算`，只有所有都为True，and运算结果才是True;
or运算是或运算，只要其中有一个为True，or运算结果就是True;
not运算是非运算，它是一个单目运算符，把True变成False，False变成True;
```
>>> True and True
True
>>> True and False
False
>>> False and False
False
>>> True or True
True
>>> True or False
True
>>> False or False
False
>>> not True
False
>>> not False
True
```

#### 空值

空值在Python里用`None`表示

### 变量

Python里的变量貌似缺少定义的过程，而且不限制数据类型，可以是任意的数据类型，命名方式与C相同：必须是大小写英文、数字和_的组合，且不能用数字开头
```
>>> a = 2
>>> b = 5
>>> c = a
>>> a = b
>>> b = c
>>> print a,b,c
5 2 2
``

### 常量

常量即不可变的量，但是Python根本没有任何机制保证常量不可变，通常使用全部大写的变量名，表示常量，而不去改变它。

### Python数据之我见

Python其实是把任何数据都当作对象，变量只是指向这些数据对象

### 字符编码

#### 字符编码

字符串也是一种数据类型，但是，字符串比较特殊的是还有一个编码问题。

因为计算机只能处理数字，如果要处理文本，就必须先把文本转换为数字才能处理。最早的计算机在设计时采用8个比特（bit）作为一个字节（byte），所以，一个字节能表示的最大的整数就是255（二进制11111111=十进制255），如果要表示更大的整数，就必须用更多的字节。比如两个字节可以表示的最大整数是65535，4个字节可以表示的最大整数是4294967295。

由于计算机是美国人发明的，因此，最早只有127个字母被编码到计算机里，也就是大小写英文字母、数字和一些符号，这个编码表被称为ASCII编码，比如大写字母A的编码是65，小写字母z的编码是122。

但是要处理中文显然一个字节是不够的，至少需要两个字节，而且还不能和ASCII编码冲突，所以，中国制定了GB2312编码，用来把中文编进去。

你可以想得到的是，全世界有上百种语言，日本把日文编到Shift_JIS里，韩国把韩文编到Euc-kr里，各国有各国的标准，就会不可避免地出现冲突，结果就是，在多语言混合的文本中，显示出来会有乱码。
因此，Unicode应运而生。Unicode把所有语言都统一到一套编码里，这样就不会再有乱码问题了。

Unicode标准也在不断发展，但最常用的是用两个字节表示一个字符（如果要用到非常偏僻的字符，就需要4个字节）。现代操作系统和大多数编程语言都直接支持Unicode。

现在，捋一捋ASCII编码和Unicode编码的区别：ASCII编码是1个字节，而Unicode编码通常是2个字节。

字母A用ASCII编码是十进制的65，二进制的01000001；

字母0用ASCII编码是十进制的48，二进制的00110000，注意字母'0'和整数0是不同的；

汉字中已经超出了ASCII编码的范围，用Unicode编码是十进制的20013，二进制的01001110 00101101。

你可以猜测，如果把ASCII编码的A用Unicode编码，只需要在前面补0就可以，因此，A的Unicode编码是00000000 01000001。

新的问题又出现了：如果统一成Unicode编码，乱码问题从此消失了。但是，如果你写的文本基本上全部是英文的话，用Unicode编码比ASCII编码需要多一倍的存储空间，在存储和传输上就十分不划算。

所以，本着节约的精神，又出现了把Unicode编码转化为“可变长编码”的UTF-8编码。UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用UTF-8编码就能节省空间：

|**字符**|**ASCII**|**Unicode**|**UTF-8**|
|---|---|---|---|
|A|01000001|00000000 01000001|01000001
|中|x|01001110|00101101	11100100|10111000 10101101|
从上面的表格还可以发现，UTF-8编码有一个额外的好处，就是ASCII编码实际上可以被看成是UTF-8编码的一部分，所以，大量只支持ASCII编码的历史遗留软件可以在UTF-8编码下继续工作。
用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件：
![](https://github.com/zt1991616/blog/raw/master/Image/14031402.png)
浏览网页的时候，服务器会把动态生成的Unicode内容转换为UTF-8再传输到浏览器：
![](https://github.com/zt1991616/blog/raw/master/Image/14031403.png)

#### Python的字符串

Python提供了ord()和chr()函数，可以把字母和对应的数字相互转换：
```
>>> ord('A')
65
>>> chr(65)
'A'
```
Python在后来添加了对Unicode的支持，以Unicode表示的字符串用u'...'表示，比如：
```
>>> print u'中文'
中文
>>> u'中'
u'\u4e2d'
```

把u'xxx'转换为UTF-8编码的'xxx'用encode('utf-8')方法：
```
>>> u'ABC'.encode('utf-8')
'ABC'
>>> u'中文'.encode('utf-8')
'\xe4\xb8\xad\xe6\x96\x87'
```

len()函数可以返回字符串的长度：
```
>>> len(u'ABC')
3
>>> len('ABC')
3
>>> len(u'中文')
2
>>> len('\xe4\xb8\xad\xe6\x96\x87')
6
```

反过来，把UTF-8编码表示的字符串'xxx'转换为Unicode字符串u'xxx'用decode('utf-8')方法：
```
>>> 'abc'.decode('utf-8')
u'abc'
>>> '\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
u'\u4e2d\u6587'
>>> print '\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
中文
```
由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：
```
# !/usr/bin/env python
#  -*- coding: utf-8 -*-
```
第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；

第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。

#### 格式化

格式化方式与C语言抑制，用`%`实现
例如：
```
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Zoe', 1000000)
'Hi, Zoe, you have $1000000.'
```
|%d|整数|
|---|---|
|%f|浮点数|
|%s|字符串|
|%x|十六进制数|
