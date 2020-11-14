title: Flutter widget
date: 2020-11-10 18:10:42
categories:
tags:
---

<!--more-->

## StatefulWidget
```dart
abstract class StatefulWidget extends Widget {
  const StatefulWidget({ Key key }) : super(key: key);

  @override
  StatefulElement createElement() => StatefulElement(this);

  @protected
  State createState();
}
```

### State
```dart
abstract class State<T extends StatefulWidget> extends Diagnosticable {
  T get widget => _widget;

  _StateLifecycle _debugLifecycleState = _StateLifeCycle.created;

  BuildContext get context => _element;

  bool get mounted => _element != null;

  void initState() {
    assert(_debugLifecycleState == _StateLifeCycle.created);
  }

  void didUpdateWidget(covariant T oldWidget) {}

  void reassemble() {}

  void setState(VoidCallback fn) {
    ...
    _element.markNeedsBuild();
  }

  void deactivate() {}

  void dispose() {
    ...
    _debugLifecycleState = _StateLifeCycle.defunct;
    ...
  }

  Widget build(BuildContext context) {}

  void didChangeDependencies() {}
}

enum _StateLifecycle {
  created; 
  initialized;
  ready;
  defunct;
}
```
State具有生命周期，`created`