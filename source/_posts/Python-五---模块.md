title: Python 五 - 模块
date: 2014-06-16 11:18:22
categories: Python
tags: [Python]
---
在Python中，一个.py文件就称为一个模块(Module)。
为了避免模块名冲突，Python按目录来组织模块，称之为包(backage)。
每个包目录下面都会有一个`__init__py`文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录，而不是一个包。`__init__.py`可以使空文件，也可以有Python代码。

##使用模块
Python本身就内置了很多非常有用的模块，只要安装完毕，这些模块就可以立刻使用。
以`sys`模块为例
```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

' a test module '
# 第一个字符串会被默认视为模块的文档说明
__author__  = 'Zoe'

import sys

def test():
	args = sys.argv
	if len(args) == 1:
		print 'Hello,world!'
	elif len(args) == 2:
		print 'Hello,%s!' % args[1]
	else:
		print 'Too many arguments!'

if __name__=='__main__':
	test()
# 当命令行运行本模块时，Python解释器吧一个特殊变量__name__置为__main__,而如果在其他地方导入该模块，if判断将会失败。
# 要导入后调用才会生效if判断
```

##别名
导入模块，还可以使用别名，这样，可以在运行时根据当前环境选择最合适的模块。
比如Python标准库一般会提供StringIO和cStringIO两个库，这两个库的接口和功能是一样的，但是cStringIO是C写的，速度更快，所以，你会经常看到这样的写法：
```Python
try:
    import cStringIO as StringIO
except ImportError: # 导入失败会捕获到ImportError
    import StringIO
```
还有类似simplejson这样的库，在Python 2.6之前是独立的第三方库，从2.6开始内置，所以，会有这样的写法：
```Python
try: import json # python >= 2.6 except ImportError: import simplejson as json # python <= 2.5
```

##作用域
在一个模块中，我们可能会定义很多函数和变量，但有的函数和变量我们不希望别人使用，有的函数和变量我们希望仅仅在模块内部使用，在Python中，是通过`_`前缀来实现的。
正常的函数和变量名是公开的(public)，可以被直接饮用。
类似__xxx__这样的变量是特殊变量，可以被直接饮用，但是有特殊用途，比如`__name__`，`__author__`，模块定义的文档注释课可以用`__doc__`访问。
类似`_xxx`和`__xxx`这样的函数或变量就是非公开的(private)，不应该被直接饮用。

##安装第三方模块
首先需要安装`setuptools`工具，Mac和Linux自带了此工具，可以在https://pypi.python.org/pypi/setuptools#window下载安装Windows版本。
安装第三方模块`easy_install XXX`即可
安装一个第三方库——Python Imaging Library，这是Python下非常强大的处理图像的工具库。一般来说，第三方库都会在Python官方的pypi.python.org网站注册，要安装一个第三方库，必须先知道该库的名称，可以在官网或者pypi上搜索，比如Python Imaging Library的名称叫PIL，因此，安装Python Imaging Library的命令就是：
```
easy_install PIL
```
##模块搜索路径
当加载一个模块时，Python会在指定的路径下搜索对应的.py文件（搜索当前目录、所有已安装的内置模块和第三方模块），如果找不到会报错。
要添加自己的搜索目录，一是直接修改模块的目录，即修改`.path`变量，而是设置在环境变量中。

##使用__future__
Python提供了__future__模块，把下一个新版本的特性导入到当前版本，于是我们就可以在当前版本中测试一些新版本的特性。举例说明如下：

为了适应Python 3.x的新的字符串的表示方法，在2.7版本的代码中，可以通过unicode_literals来使用Python 3.x的新的语法：
```Python
# still running on Python 2.7

from __future__ import unicode_literals

print '\'xxx\' is unicode?', isinstance('xxx', unicode)
print 'u\'xxx\' is unicode?', isinstance(u'xxx', unicode)
print '\'xxx\' is str?', isinstance('xxx', str)
print 'b\'xxx\' is str?', isinstance(b'xxx', str)
```