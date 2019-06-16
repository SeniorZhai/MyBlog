title: Dart 基本
date: 2019-06-07 14:11:21
categories: Flutter
tags: [dart]

---

## 内置的类型

- numbers
- strings
- booleans
- lists
- maps
- runes
- symbols
  <!--more-->

### numbers 数值

数字类型的父类，有两个子类`int`和`double`

### string 字符串

Dart 字符串是 UTF-16 编码的字符序列，可以使用点引号和双引号，而且可以嵌套。
使用`\`进行转义
使用三个单引号或者双引号可以创建多行字符串对象
使用`r`前缀可以创建原始字符串(不转义)

```dart
print(r"换行符\n")
```

### booleans

布尔值可以是`false`和`true`，类型名为`bool`

## lists

```dart
var list = [1,2,3];
var list = new List(1);
var list = const [1,2,3]; // 定义不可变List
```

### maps

```dart
var map = {'a':1,'b':2};
var map = new Map();
map['a'] = 1;
var c = const {'a':1,'b':2};
```

### runes(Unicode 字符)

```dart
var clapping = '\u{1f44f}' //使5位16进制
```

### symbols

```dart
var a = #A; // 常量
var b = new Symbol("A");
print(a == b); //true
```

## 变量定义

```dart
Object a = 'String'; // 可以是任意类型
var a = 1; // 赋值 确定类型
var b; // dynamic 没有赋值可以是任意类型
b = 1;
b = 'a';

// 常量
const a = 1; // 编译时常量 效率更高
final b = 1; // 运行时常量
```

## 操作符

| 操作符    | 说明               |
| :-------- | :----------------- |
| as        | 类型转换           |
| is        | 判断类型           |
| is!       | 判断类型与 is 相反 |
| =         | 赋值               |
| += \= \*= | 运算并赋值         |
| ??=       | 为 null 赋值       |

```dart
a ??= value;
// 如果 a==null,a=value
// 否则 a不变
```

### 条件表达式

- condition ? expr1 : expr2
  如果 condition==true 执行返回 expr1，否则执行返回 expr2
- expr1 ?? expr2
  如果 expr1 不为空返回 expr1 否则返回 expr2

### 级联操作符

`..`可以在同一个对象上连续调用多个函数活访问变量

```dart
var sb = new StringBuffer();
sb..write('foo')..write('bar');
```

### 空安全操作符

```dart
String sb;
sb?.length // null
```
