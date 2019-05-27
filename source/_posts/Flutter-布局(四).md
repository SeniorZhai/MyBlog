title: Flutter 布局(四)
date: 2019-05-26 12:37:36
categories: Flutter
tags: [iOS,Android]

层叠布局，类似 Android 的 FrameLayout，根据父容器的四个角来确定本身的位置

---

Flutter 中 Stack 实现堆叠，Positioned 实现对 Widget 定位

## Stack

- alignment 如何对齐 left、right、top、bottom
- textDirection 对齐的参考系
- fit 没有定位的 Children 如何适应大小 StackFit.loose 表示使用 Widget 大小，StackFit.expand 表示扩展到 Stack 的大小
- overflow 决定超出容器的 Children 如何显示，Overflow.clip 超出部分被剪裁，Overflow.visible 显示超出的 Children

## Positioned

left、top、right、bottom 表示离 Stack 四边的距离，width、height 表示定位元素的长宽

```dart
ConstrainedBox(
  constraints: BoxConstraints.expand(),      child: Stack(
    overflow: Overflow.visible,
    alignment: Alignment.topLeft,
    children: <Widget>[
      Ball(30, Colors.blue),
      Positioned(
        bottom: 20,
        child: Ball(35, Colors.red),
      )],
  )
)
```
