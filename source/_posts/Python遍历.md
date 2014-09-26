title: Python遍历
date: 2014-09-27 15:24:13
categories: Python
tags: [遍历]
---
模块(module)可以更好的组织已有的程序
<!--more-->
##引入模块
```python
import first	# 引入first模块
# ...
import a as b 	# 引入a模块并重命名为b
from a import function1	#引入a模块中的function1对象
from a import * # 引入a模块中的所有对象
```
##搜索路径
1. 程序所在的文件夹
2. 标准库的安装路径
3. 操作系统环境中PYTHONPATH所包含的路径
##模块包
可以把功能相似的模块放在同一个文件夹下，通过`import this_dir.module`引入thisdir文件夹下的module模块
该文件夹中必须包含一个`__init__.py`的文件来提醒Python该文件夹为一个模块包，`__init__.py`可以是一个空文件
