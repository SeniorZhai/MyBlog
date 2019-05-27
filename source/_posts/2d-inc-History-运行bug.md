title: 2d-inc History 运行 bug
date: 2019-05-23 13:17:23
categories: Flutter
tags: [Android,iOS,flare]

---

运行[HistoryOfEverything](https://github.com/2d-inc/HistoryOfEverything)项目时，报错

```
Try changing the type of the parameter, or casting the argument to 'Uint16List'.
    indices: _indices, textureCoordinates: _uvBuffer);
```

<!--more-->

将此[这里](https://github.com/2d-inc/Nima-Flutter/blob/85493feedee43aae7421f1c9cca76d853b1cb2da/lib/nima.dart#L20)和[这里](https://github.com/2d-inc/Nima-Flutter/blob/85493feedee43aae7421f1c9cca76d853b1cb2da/lib/nima.dart#L65)的`Int32List`修改为`Uint16List`
