title: Flutter动画
date: 2020-09-23 20:44:45
categories:
tags:
---

<!--more-->
## AnimationController
```dart
AnimationController(vsync:this, duration: Duration(milliseconds: 5000));
```
- `vsync` 防止屏幕外动画消耗不必要的资源 单个AnimationController使用SingleTickerProviderStateMixin，多个AnimationController使用TickerProviderStateMixin
- `duration` 动画执行的时间
```dart
class _AnimationBaseDemoState extends State<AnimationBaseDemo> with SingleTickerProviderStateMixin {
 double _size = 100;
  AnimationController _controller;
  @override
  void initState() {
    super.initState();
    _controller = AnimationController(vsync: this,duration: Duration(milliseconds: 500),lowerBound: 100,upperBound: 200)
    ..addListener(() {
      setState(() {
        _size = _controller.value;
      });
    })
  }

  @override
  Widget build(BuildContext context) {
    return Center(
      child: GestureDetector(
        onTap: () {
          _controller.forward();
        },
        child: Container(
          height: _size,
          width: _size,
          color: Colors.blue,
          alignment: Alignment.center,
          child: Text('点我变大',style: TextStyle(color: Colors.white,fontSize: 18),),
        ),
      ),
    );
  }
  
  @override
  void dispose() {
    super.dispose();
    _controller.dispose();
  }
}
```
- `dismissed` 动画停止在开始处
- `forward` 动画从开始处运行到结束处
- `reverse` 动画充结束处运行到开始处
- `completed` 动画结束在借书处
- `repeat` 重复执行动画
- `reset` 重置动画
### Tween
动画映射
```dart
AnimationController _controller;
Animation<Color> _animation;

@override
void initState() {
  super.initState();
    _controller = AnimationController(vsync: this, duration: Duration(milliseconds: 500));
    _animation = ColorTween(begin: Colors.blue, end: Colors.red).animate(_controller);
}
```

### Curve
动画曲线
```dart
 super.initState();
    _controller =
        AnimationController(vsync: this, duration: Duration(milliseconds: 1000))
          ..addListener(() {
            setState(() {});
          });

    _animation = Tween(begin: 100.0, end: 200.0)
        .chain(CurveTween(curve: Curves.bounceIn))
        .animate(_controller);
  }
```
### 自定义曲线
```dart
class _StairsCurve extends Curve {
  final int num;
  double _perStairY;
  double _perStairX;

  _StairsCurve(this.num) {
    _perStairY = 1.0 / (num - 1);
    _perStairX = 1.0 / num;
  }

  @override
  double transformInternal(double t) {
    return _perStairY * (t / _perStairX).floor();
  }
}
```