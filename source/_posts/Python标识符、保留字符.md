title: Python标识符、保留字符
date: 2014-12-04 20:44:40
categories: Python
tags: 基础
---
<!--more-->
## 标识符
在Python中标识符由字母、数字、下划线组成，但不能以数字开头
标识符区分大小写
以下划线开头的标识符是有特殊意义的，以单下划线开头(`_foo`)表示不能直接访问的类属性，需要通过类提供的借口进行访问
以双下划线开头的(`__foo`)代表类的私有成员，以双下划线开头和结尾的(`__foo__`)表示Python里特殊方法，如`__init__()`表示类的构造函数
## 保留字符
|||||
|:--|:--|:--|
|and|exec|not|
|assert|finally|or|
|break|for|pass|
|class|from|print|
|continue|global|raise|
|def|if|return|
|del|import|try|
|elif|in|while|
|else|is|with|
|except|lambda|yield|
|and|exec|not|
|assert|finally|or|
|break|for|pass|
|class|from|print|
|continue|global|raise|
|def|if|return|
|del|import|try|
|elif|in|while|
|else|is|with|
|except|lambda|yield|
## 变量类型
- Numbers(数字)
	+ int(有符号整形)
	+ long(长整型)
	+ float(浮点数)
	+ comples(复数) a+bj或者comples(a,b)表示
- String(字符串)
- List(列表)
- Tuple(元组)
- Dictionary(字典)

### 类型转换
|函数|描述|
|:--|:--|
|int(x[,base])|将x转换为一个整数|
|long(x[,base])|将x装换成一个长整型|
|float(x)|将x转换到一个浮点数|
|complex(real [,imag])|创建一个复数|
|str(x)|将对象 x 转换为字符串|
|repr(x)|将对象 x 转换为表达式字符串|
|eval(str)|用来计算在字符串中的有效Python表达式,并返回一个对象|
|tuple(s)|将序列 s 转换为一个元组|
|list(s)|将序列 s 转换为一个列表|
|set(s)|转换为可变集合|
|dict(d)|创建一个字典。d 必须是一个序列 (key,value)元组|
|frozenset(s)|转换为不可变集合|
|chr(x)|将一个整数转换为一个字符|
|unichr(x)|将一个整数转换为Unicode字符|
|ord(x)|将一个字符转换为它的整数值|
|hex(x)|将一个整数转换为一个十六进制字符串|
|oct(x)|将一个整数转换为一个八进制字符串|

## 算术运算符
|运算符|描述|
|:--|:--|
|+|加|
|-|减|
|*|乘|
|/|除|
|%|取模|
|**|幂|
|//|取整除|

## 比较运算符
|运算符|描述|
|:--|:--|
|==|等于|
|!=|不等于|
|<>|不等于|
|>|大于|
|<|小于|
|>=|大于等于|
|<=|小于等于|

## 成员运算符
|运算符|描述|
|:--|:--|
|in|是否在指定序列中|
|not in|是否不再指定序列中|

## 身份运算符
|运算符|描述|
|:--|:--|
|is|判断两个变量是不是引用自一个对象|
|is not|判断两个变量是不是引用不同的对象|