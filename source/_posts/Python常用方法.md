title: Python常用方法
date: 2014-09-26 08:24:13
categories: Python
tags: [常用]
---
<!--more-->
## 内置方法
- type()	查看数据类型
- dir()		返回所以属性和方法
- help() 	返回帮助信息
- range(x,y)	获取x到y-1的元素
## 列表

### 常用方法
- count()	列表含多少元素
- index(x)	差选下标为x的元素
- append()	在末尾添加元素
- sort()	排序
- pop()		或许最后一个元素，并去除
- remove()	删除元素
- insert(x,y)	在下边x的位置插入y

## 字典

### 常用方法
- keys()	返回dict所有键
- values()	返回dict所有值
- items()	返回dict所有元素
- clear()	清空dict
- del dic{'tom'}	删除'tom'元素
- len()			获取地点的元素总数

```python
dic = {'zoe':1,'joy':2,'tom':3}
print dic
print len(dic)
print dic.keys()
print dic.values()
print dic.items()
del dic['tom']
print dic
dic.clear()
print dic
```