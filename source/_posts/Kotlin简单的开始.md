title: Kotlin简单的开始
date: 2016-02-18 17:23:49
categories: Android
tags:[Kotlin]
---
##安装Kotlin插件
- Kotlin 使Android Studio可以识别kotlin代码
- Kotlin Android Extensions 使Android Studio可以自动地从XML中注入所有的View到Activity

##修改Gradle
```Gradle
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
android {
  ...
  sourceSets {
    main.java.srcDirs += 'src/main/kotlin'
  }
}

dependencies {
  ...    
  compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
}

buildscript {
    ext.kotlin_version = '1.0.0'
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}
repositories {
    mavenCentral()
}
```
PS：未修改处省略

##修改MainActivity
选择MainActivity.java->Code->Convert Java File to Kotlin File
