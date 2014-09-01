title: Eclipse导入Gradle项目
date: 2014-07-02 13:32:22
categories: Android
tags: [Android]
---
[Importing gradle project to eclipse](http://stackoverflow.com/questions/20805715/importing-gradle-project-to-eclipse)

1. 打开`build.gradle`文件，在第一行添加以下代码
```
apply plugin 'eclipse'
```
2. 在项目所在目录下运行以下命令
Windows下
```
gradlew.bat eclipse
```
mac或linux下
```
./gradlew eclipse
```
3. 最后导入Eclipse中即可