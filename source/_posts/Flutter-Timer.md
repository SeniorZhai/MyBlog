title: Flutter Timer
date: 2020-09-23 14:03:50
categories:
tags:
---

<!--more-->
```dart
int countValue = 10;
Timer _timer;

void startTimer() {
  final Duration duration = Duration(seconds: 1);
  cancelTimer();

  _timer = Timer.periodic(duration, (timer) {
    countValue = countValue - 1;
    if (this.mounted) {
      setState((){
        //..
      });
    }
    print(countValue);
    if (countValue <= 0) {
      cancelTimer();
    }
  });
}
```
## 常见问题
1. Widget销毁时取消Timer
```dart
@override
void dispose() {
  cancelTimer();
  super.dispose();
}
```
2. 生命周期变动
```dart
@override
void didChangeAppLifecycleState(AppLifecycleState state) {
  switch(state) {
    case AppLifecycleState.resumed: {
      break;
    }
    case AppLifecycleState.paused: {
      cancelTimer();
      break;
    }
    case AppLifecycleState.inactive: {
      cancelTimer();
      break;
    }
    case AppLifecycleState.detached: {
      cancelTimer();
      break;
    }
  }
  super.didChangeAppLifecycleState(state);
}
```
