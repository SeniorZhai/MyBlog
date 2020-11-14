title: Flutter 布局(三)
date: 2019-05-25 23:47:36
categories: Flutter
tags: [iOS,Android]

---

流式布局，子 Widget 超出屏幕会自动折行

<!--more-->

## Wrap

Wrap 和 Row 一样包含 direction、crossAxisAlignment、textDirection、verticalDirection 属性，作用相同，多出下面几个属性

- spacing 主轴方向 ziWidget 间距
- runSpacing 纵轴方向间距
- runAlignment 纵轴方向对齐方式

## Flow

Flow 比较复杂，通常用在自定义布局或者性能要求较高的场景，Flow 拥有以下优点

- 性能好 Flow 是一个队 child 尺寸以及位置调整非常高效的控件。用转换矩阵在对 child 进行位置的调整进行优化，Flow 定位过后，如果 child 尺寸或者位置发生了变化，在 FlowDlegate 中的 painChildren()方法调用`context.paintChild`进行重绘，并没有实际调整 Widget 位置
- 灵活 需要实现 FlowDelegate 的`paintChildren()`方法，需要计算 children 的位置，自定义布局策略

缺点

- 复杂
- 不能自适应子 Widget 大小，必须通过制定父容器的大小或者实现 FlowDelegate 的`getSize`返回固定大小

简单实现流式布局

```dart
class FlowPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Flow'),
      ),
      body: Flow(
        delegate: MyFlowDelegate(),
        children: <Widget>[
          Ball(80, Colors.cyan),
          Ball(70, Colors.purple),
          Ball(90, Colors.grey),
          Ball(50, Colors.red),
          Ball(10, Colors.green),
          Ball(20, Colors.black),
          Ball(40, Colors.blue),
          Ball(50, Colors.pink),
          Ball(100, Colors.orange),
        ],
      ),
    );
  }
}

class MyFlowDelegate extends FlowDelegate {
  @override
  void paintChildren(FlowPaintingContext context) {
    var x = 0.0;
    var y = 0.0;
    var maxHeight = 0.0;
    for (int i = 0; i < context.childCount; i++) {
      var w = context.getChildSize(i).width + x;
      var h = context.getChildSize(i).height;
      if (w < context.size.width) {
        // 小于宽之间排在同一行
        context.paintChild(i,
            transform: new Matrix4.translationValues(x, y, 0.0));
        x = w;
        maxHeight = max(maxHeight, h);
      } else {
        x = 0.0;
        y += maxHeight;
        //绘制子widget(有优化)
        context.paintChild(i,
            transform: new Matrix4.translationValues(x, y, 0.0));
        x += context.getChildSize(i).width;
      }
    }
  }

  @override
  Size getSize(BoxConstraints constraints) {
    return super.getSize(constraints);
  }

  @override
  bool shouldRepaint(FlowDelegate oldDelegate) {
    return oldDelegate != this;
  }
}

class Ball extends StatelessWidget {
  double _size;
  Color _color;

  Ball(double size, Color color) {
    this._size = size;
    this._color = color;
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      width: _size,
      height: _size,
      decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(_size / 2), color: _color),
    );
  }
}
```
