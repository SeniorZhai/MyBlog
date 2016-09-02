title: Android Lint工具
date: 2015-08-12 17:51:04
categories: Android
tags: [Lint]
---
<!--more-->
Android Lint是SDK Tools 16(ADT 16)之后Google提供的新工具，它是一个代码扫描工具，能够帮助我们识别代码结构存在的问题，比如：
1. 无效布局，多重嵌套等
2. 未使用的冗余资源文件
3. 国际化的问题(未翻译的文本)
4. 数组资源大小不一致
5. 图标缺失、重复、错误尺寸
6. AndroidManifest.xml中的错误

## Lint
Lint工具在`/AndroidSDK/tools/`文件夹下

### 使用
- `lint <project directory>` lint命令后添加工程目录
- `lint --html <project directory>` 生成html格式的报告
- `lint --simplehtml <project directory>` 生成简单html格式的报告
- `lint --xml <project directory>` 生成xml格式的报告
- `lint --check "UnusedResources" <project directory>` 清理冗余资源文件
- `lint --show`可获得详细问题列表
	+ AdapterViewChildren 确保没有在XML文件中定义它的子view
	+ OnClick 确保XML文件声明的OnClick的调用函数在代码中实际存在
	+ SuspiciousImport 可疑import的检查
	+ UsesMinSdkAttributes 检查是否在AndroidManifest.xml文件中定义了minimum SDK 和 target SDK这两个属性
	+ WrongViewCast View强转换的检查
	+ MissingRegistered 检查manifest文件中声明的类实际是否存在于工程或工程的libraries中
	+ NamespaceTypo 命名空间拼写检查
	+ Proguard 混淆配置检查
	+ ScrollViewCount 检查ScrollViews是否只有一个child
	+ ……