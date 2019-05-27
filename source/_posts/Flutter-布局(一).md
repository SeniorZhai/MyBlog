title: Flutter 布局(一)
date: 2019-05-25 23:07:36
categories: Flutter
tags: [iOS,Android]

---

布局类的 Widget 包含一个活多个子 Widget，不同的布局会用不同方式对子 Widget 进行排版

<!--more-->

| Widget                        | Element                        | 作用                                            |
| :---------------------------- | :----------------------------- | :---------------------------------------------- |
| LeafRenderObjectWidget        | LeafRenderObjectElement        | 没有子 Widget 的 Widget(Text、Image……)          |
| SingleChildRenderObjectWidget | SingleChildRenderObjectElement | 包含一个 Widget(ConstrainedBox、DecoratedBox……) |
| MultiChildRenderObjectWidget  | MultiChildRenderObjectElement  | 拥有多个子 Widget(Row、Column……)                |

布局类就是直接、间接继承`MultiChildRenderObjectWidget`的 Widget，它们一般都会有一个 children 属性接收子 Widget。

## 线性布局 Row Column

沿水平或垂直方向排列子 Widget 的布局 Widget

### 概念

- 主轴 与主方向同方向
- 纵轴 与主方向垂直

#### Row

- textDirection 水平布局顺序 (从左到右或者从右到左)
- mainAxisSize 主轴占用的空间，默认占据所有空间(MainAxisSize.max)
- mainAxisAlignment 水平方向占据空间的排列方式
- verticalDirection 纵轴对齐方式（从上到下或者从下到上）
- crossAxisAlignment 纵轴对齐方式
- children 子 Widgets 数组

#### Column

与 Row 正好垂直，纵向布局

> 如果 Row 嵌套 Row，Column 嵌套 Column，如果父 Row(Column)占据最大空间，子 Row(Column)会占据实际大小(mainAxisSize 无效)
