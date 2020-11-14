title: Flutter 引入到现有项目
date: 2019-05-19 17:07:55
categories: Flutter
tags: [iOS,Android,desktop]

---

<!--more-->

## Android

在 Android 项目中调用 Flutter，`flutter`将作为一个`Module`被元工程引用，为了方便管理，我将`flutter`项目的工程和`Android`工程放在同一层级
创建`flutter module`

```
flutter create -t module flutter_demo
```

在 Android 工程项目中的`settings.gradle`中添加

```gradle
setBinding(new Binding([gradle: this]))
evaluate(new File(
        settingsDir,
        '../flutter_demo/.android/include_flutter.groovy'))
```

在需要引用的 App Module 添加依赖

```gradle
dependencies {
  implementation project(':flutter')
}
```

> 注意：需要将 App 的 sdk:minSdkVersion 和 flutter 的设置一致，同时需要 flutter android 工程中可能还在引用 support 的包，可以升级为 androidx.appcompat

[示例](https://github.com/SeniorZhai/Hybrid)
