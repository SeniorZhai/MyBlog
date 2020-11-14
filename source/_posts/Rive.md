title: Rive
date: 2020-11-09 23:31:25
categories: [flutter]
tags: [flutter, Rive]
---

Rive是一个flutter的动画引擎
<!--more-->

## 如何使用
添加`rive`到**pubspec.yaml**
```yaml
dependencies:
  rive: ^0.6.2
flutter:
  assets:
    - assets/truck.riv
```

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:rive/rive.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return MaterialApp(
      title:'Rive Flutter Deme',
      home: Scaffold(
        body: MyRiveAnimation()
      ),
    );
  }
}

class MyRiveAnimation extends StatefulWidget { 
  @override
  _MyRiveAnimationState createState() => _MyRiveAnimationState();
}

class _MyRiveAnimationState extends State<MyRiveAnimation> {
  final riveFileName = 'assets/truck.riv';
  Artboard _artboard;
  
  @override
  void initState() {
    _loadRiveFile();
    super.initState();
  }

  void _loadRiveFile() async {
    final byte = await rootBundle.load(riveFileName);
    final file = RiveFile();
    if(file.import(bytes)) {
      setState(()=> _artboard = file.mainArtborad
        ..addController(
          SimpleAnimation('idle'),
        ));
    }
  }

}
```