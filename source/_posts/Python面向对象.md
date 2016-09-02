title: Python面向对象
date: 2014-12-05 14:28:29
categories: Python
tags: [面向对象]
---
<!--more-->
- 类(class) 用来描述具体相同的属性和方法的对象集合
- 类变量 类内部的公有变量
- 数据成员 类变量或者实例变量用于处理类及其实例对象的相关的数据
- 方法重载(override) 父类继承的方法不能满足子类的需求，可以对其进行修改
- 实例变量 在方法中定义的变量，只作用于当前实例的类
- 继承 派生类继承基类
- 实例化 创建一个类的实例
- 方法 类中定义的函数
- 对象 通过类定义的数据结构实例

## 创建类
```python
class ClassName:
	'Optional class documentation string' #  类文档字符串
	class_suite #  类体
```
- getattr(obj,name[,default]):访问对象的属性
- hasattr(obj,name):检查是否存在一个属性
- setattr(obj,name,value):设置一个属性，如果不存在会创建一个新的属性
- delattr(obj,name):删除属性

## Python内置类属性
- `__dict__`:类的属性(包含一个字典)
- `__doc__`:类的文档字符串
- `__name__`:类名
- `__module__`:类定义所在的模块(类的全名是`__main__.className`)
- `__base__`:类的所有父类构成元素

## Python对象销毁
在Python内部记录着所有使用对象各有多少引用，一个内部跟踪变量，称为一个引用计数器
析构函数`__del__`在对象消逝的时候被调用

