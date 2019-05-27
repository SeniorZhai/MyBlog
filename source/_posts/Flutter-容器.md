title: Flutter 容器
date: 2019-05-26 13:47:36
categories: Flutter
tags: [iOS,Android]

---

容器 Widget 通常只接受一个子 Widget，他们直接过着间接的继承 SingleChildRenderObjectWidget

容器 Widget 一般用于包装子 Widget，添加一些修饰、变换、限制等

<!--more-->

## Padding

给子 Widget 添加留白

- padding 留白的大小(EdgeInsetsGeometry 抽象类)，通常使用 EdgeInsets
  - EdgeInsets.fromLTRB(double left, double top, double right, double bottom) 分别设置四个方向 padding
  - EdgeInsets.all(double value) 用相同数值设置四边的 padding
  - EdgeInsets.only({left, top, right, bottom}) 单独一个方向的 padding
  - EdgeInsets.symmetric({vertical, horizontal}) 设置上下或者左右 padding

## ConstrainedBox

ConstrainedBox 用于对子 widget 添加额外的约束

```dart
  ConstrainedBox(
    constraints: BoxConstraints(),
  )
```

BoxConstraints 限制最大最小的宽高

```dart
const BoxConstraints({
  this.minWidth = 0.0, //最小宽度
  this.maxWidth = double.infinity, //最大宽度
  this.minHeight = 0.0, //最小高度
  this.maxHeight = double.infinity //最大高度
})
```

## SizedBox

用于设置子 Widget 固定宽高

```dart
SizedBox(
  width: 80.0,
  height: 80.0,
  child: childWidget //
)
```

## DecoratedBox

用于对子 Widget 绘制一个装饰

```dart
const DecoratedBox({
  Decoration decoration,
  DecorationPosition position = DecorationPosition.background,
  Widget child
})
```

- decoration Decoration 实现 createBoxPainter 绘制装饰
- position 绘制的位置
  - background 背景
  - foreground 前景

### BoxDecoration

BoxDecoration 是 DecoratedBox 的子类

```dart
BoxDecoration({
  Color color, //颜色
  DecorationImage image,//图片
  BoxBorder border, //边框
  BorderRadiusGeometry borderRadius, //圆角
  List<BoxShadow> boxShadow, //阴影,可以指定多个
  Gradient gradient, //渐变
  BlendMode backgroundBlendMode, //背景混合模式
  BoxShape shape = BoxShape.rectangle, //形状
})
```

## Transform

可以对子 Widget 进行矩阵变换

```dart
Container(
  color: Colors.black,
  child: new Transform(
    alignment: Alignment.topRight, //相对于坐标系原点的对齐方式
    transform: new Matrix4.skewY(0.3), //沿Y轴倾斜0.3弧度
    child: new Container(
      padding: const EdgeInsets.all(8.0),
      color: Colors.deepOrange,
      child: const Text('Apartment for rent!'),
    ),
  ),
);
```

### 平移

`Transform.translate`接收 offset 参数，沿 x、y 轴进行平移

### 旋转

`Transform.rotate`旋转一个角度

### 缩放

`Transform.scale`可以对子 Widget 进行缩小或放大

## RotatedBox

与`Transform.rotate`功能相同，Rotated 的旋转在 layout 阶段，Transform 作用在绘制阶段

## Container

综合功能的容器 Widget

```dart
Container({
  this.alignment,
  this.padding,
  Color color, // 背景色
  Decoration decoration, // 背景装饰
  Decoration foregroundDecoration, //前景装饰
  double width,//容器的宽度
  double height, //容器的高度
  BoxConstraints constraints, //容器大小的限制条件
  this.margin,//容器外补白，不属于decoration的装饰范围
  this.transform, //变换
  this.child,
})
```

## Margin

外边距容器

## Scaffold

Meaterial 标准页面
包含可选的`appBar`、`drawer`、`bottomNavigationBar`、`floatingActionButton`等

### AppBar

Material 风格的导航栏

```dart
AppBar({
  Key key,
  this.leading, //导航栏最左侧Widget，常见为抽屉菜单按钮或返回按钮。
  this.automaticallyImplyLeading = true, //如果leading为null，是否自动实现默认的leading按钮
  this.title,// 页面标题
  this.actions, // 导航栏右侧菜单
  this.bottom, // 导航栏底部菜单，通常为Tab按钮组
  this.elevation = 4.0, // 导航栏阴影
  this.centerTitle, //标题是否居中
  this.backgroundColor,
  ...   //其它属性见源码注释
})
```

### Drawer

抽屉，类似 DrawerLayout

### FloatingActionButton

类似，FloatingActionButton
