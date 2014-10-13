title: Python小技巧
date: 2014-10-13 11:09:07
categories: Python
tags: [Python]
---
<!--more-->
##交换变量
```python
x = 6
y = 5
x,y = y,x
```
##行内if语句
```python
print "Hello" if True else "World"	# Hello
```
##连接
不同类型的对象连接
```python
nfc = ["Packers", "49ers"]
afc = ["Ravens", "Patriots"]
nfc + afc # ['Packers', '49ers', 'Ravens', 'Patriots']
print str(1) + " world"
>>> 1 world
 
print `1` + " world"
>>> 1 world

print 1, "world"
>>> 1 world
print nfc, 1
>>> ['Packers', '49ers'] 1
```
##数字技巧
```python
5.0//2 	# 向下取整 2
2**5 	# 2的5次方
.3/.1	# 2.9999999999999996
.3//.1	# 2.0
```
##数值比较
```python
x = 2
if 3 > x > 1
	print x
if 1 < x > 0
	print x
```
##同时迭代两个列表
```python
nfc = ["Packers", "49ers"]
afc = ["Ravens", "Patriots"]
for teama, teamb in zip(nfc, afc):
     print teama + " vs. " + teamb
```
##带索引的列表迭代
```python
teams = ["Packers", "49ers", "Ravens", "Patriots"]
for index, team in enumerate(teams):
    print index, team
```
##列表推导式
```python
numbers = [1,2,3,4,5,6]
even = [number for number in numbers if number%2 == 0] # 取偶数
```
##字典推导
```python
teams = ["Packers", "49ers", "Ravens", "Patriots"]
print {key: value for value, key in enumerate(teams)}
```
##初始化列表
```python
items = [0]*3
```
##列表转换成字符串
```python
teams = ["Packers", "49ers", "Ravens", "Patriots"]
print ", ".join(teams)
>>> 'Packers, 49ers, Ravens, Patriots'
```