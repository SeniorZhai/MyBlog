title: Flutter desktop 迁移到原有项目
date: 2019-05-16 15:55:28
categories: Flutter
tags: [iOS,Android,desktop]

---

Flutter create 现在还不支持 desktop，想要在原有的 Flutter 项目支持 Desktop，需要以下步骤

<!--more-->

复制[Flutter desktop](https://github.com/google/flutter-desktop-embedding)的`example`下`macos`文件夹到工程目录

对`pubspec.yaml`文件进行修改

```yaml
environment:
  sdk: ">=2.1.0 <3.0.0"
 flutter: ">=1.5.9-pre.252" // 添加
 dependencies:
   flutter:
     sdk: flutter
  intl: 0.15.8 // 升级
```

对`main.dart`进行修改

```dart
import 'dart:io' show Platform;

import 'package:flutter/foundation.dart'
    show debugDefaultTargetPlatformOverride;

void main() {
  debugDefaultTargetPlatformOverride = TargetPlatform.fuchsia;
  runApp(MyApp());
}
```

运行

```shell
> flutter packages get
> flutter run
```
