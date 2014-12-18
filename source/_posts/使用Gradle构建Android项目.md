title: 使用Gradle构建Android项目
date: 2014-12-09 13:48:44
categories: Android
tags: [Gradle,Android Studio]
---
<!--more-->
##Gradle
Gradle以Groovy为基础，面向Java应用，基于DSL语法的自动化构建工具

##Gradle基本结构
```groovy
buildscript {
    repositories {
       mavenCentral()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.0.0'
    }
}

apply plugin: 'android'
android{
	compileSdkVersion 19
	buildToolsVersion "19.0.0"
}
```
- buildscript{...} 设置脚本的运行环境
- apply plugin: 'android' 设置使用android插件构建项目
- android{...} 设置编译android项目的参数

##任务task执行
通常的任务有
1. 输出一个项目文件，Android就是打包APK
2. 运行检查，检查程序的错误，语法等
3. 执行`assemble`和`check`
4. 清理项目输出文件

##基本的构建定制
支持的配置有
- minSdkVersion 最小支持sdk版本
- targetSdkVersion 编译时目标的sdk版本
- versionCode 程序版本号
- versionName 程序版本名称
- applicationId 程序包名
- package Name for the test application 测试用的程序包名
- instrumentation test runner 测试用的instrumentation实例
```groovy
android{
	compileSdkVersion 19
	buildToolsVersion "19.0.0"

	defaultConfig{
		versionCode 12
		versionName "2.0"
		minSdkVersion 16
		targetSdkVersion 16
	}
}
```

##签名配置
```groovy
android{
	signingConfigs {
		debug{
			storeFile file("debug.keystore")
		}

		myConfig{
			storeFile file("other.keystore")
			storePassword "android"
			keyAlias "androiddebugkey"
			keyPassword "android"
		}
	}

	buildTypes {
		foo {
			debuggable true
			jinDebugBuild true
			signingConfig signingConfigs.myConfig
		}
	}
}
```

##设置代码混淆
```groovy
android {
	buildTypes{
		release {
			runProguard true
			proguardFile getDefaultProguardFile('poguard-android.txt')
		}
	}
}
```

##依赖配置
```groovy
dependencies{
	compile files('libs/foo.jar')	// 单个文件
	compile fileTree(dir:'libs',include:['*.jar']) // 多个文件
}
```
maven仓库
```groovy
repostiories {
	mavenCentral()
}

dependencies {
	complie 'com.google.guava:guava:11.0.2'
}

android {
	...
}
```

