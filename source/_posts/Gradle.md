title: Gradle
date: 2014-06-24 13:30:16
categories: Android
tags: [Android]
---
##Gradle
- 什么是Gradle？
Gradle 是以 Groovy 语言为基础，面向Java应用为主，基于DSL语法的自动化构建工具。说到Java的自动化构建工具。
使用Gradle构建Android项目的优点：
- 在IDE环境和命令行下使用同一个构建系统
- 改进的依赖关系管理
- 更容易地集成到自动构建系统

##Gradle 基本概念
如果你用Android Studio新建一个项目的时候，默认生成一大堆关于gradle的东西，其中最重要的是一个build.gradle的文件，内容如下：
```Groovy
buildscript {
    // 声明用本地maven库查找依赖
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:0.8.+'
    }
}
// 声明项目是一个android构建
apply plugin: 'android'
// SDK版本 build工具版本
android {
    compileSdkVersion 19
    buildToolsVersion "19.0.0"
    // 版本细节
    defaultConfig {
        minSdkVersion 14
        targetSdkVersion 19
        versionCode 1
        versionName "1.0"
    }
    // 发行版定义
    buildTypes {
        release {
            runProguard false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.txt'
        }
    }
}

dependencies {
    compile 'com.android.support:support-v4:19.0.+'
}
```
buildscript节点的内容完全不用动，大概意思就是支持maven，声明Gradle的版本。
apply plugin节点声明构建的项目类型，这里当然是android了
android节点设置编译android项目的参数，接下来，我们的构建android项目的所有配置都在这里完成。

##构建一个Gradle Android项目
除了最基本的Android Gradle配置文件，项目常常需要引入第三方的jar包，比如依赖一个support_v4的jar包，则完整的build.gradle文件如下：
```Groovy
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:0.8.+'
    }
}
apply plugin: 'android'

android {
    compileSdkVersion 19
    buildToolsVersion "19.0.0"

    defaultConfig {
        minSdkVersion 14
        targetSdkVersion 19
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            runProguard false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.txt'
        }
    }
}

dependencies {
	//单文件依赖
    compile files('libs/android-support-v4.jar")
    //某个文件夹下面全部依赖
    compile fileTree(dir: 'libs', include: '*.jar')
}
```
接着在命令行切换到项目目录下
```
gradle clean
```
如果是第一次使用gradle构建，则会下载相关依赖包并且对环境进行初始化，如果出错了，一般可能是下载超时，试多几次即可，最后你会看到如下提示：BUILD SUCCESSFUL 完成以上的步骤，就可以正式使用gralde 构建你的android项目了。

接着执行
```
gradle build
```
就完成了android 项目的构建了。如果，你是照着以上步骤走的话，你将会在项目目录里面看到一个build 的目录，里面就是用gradle 构建android项目的全部东西了。最终打包的apk 就在build/apk 目录下了。然后，你会发现，两个apk 一个是 [项目名]-debug-unaligned [项目名]-release-unsigned，看名字就猜到一个是调试模式没有进行优化的apk（可直接安装），一个是release模式但没有签名的apk（不可直接安装）。

##打包签名
默认输出 release apk 是没有签名的，那么我们需要签名的很简单，只需要在android{}里面补充加上如下代码即可。
```
// 签名
signingConfigs {
    myConfig {
        storeFile file("storm.keystore")
        storePassword "storm"
        keyAlias "storm"
        keyPassword "storm"
    }
}
    
buildTypes{
    release {
        signingConfig  signingConfigs.myConfig
        runProguard true
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.txt'
    } 
}
```
然后，运行
```
gradle clean 
gradle build 
```
接着在build/apk目录下就生成了我们需要的签名的apk。