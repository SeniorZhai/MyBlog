title: 我眼中的REST客户端(一)
date: 2014-09-15 16:18:47
categories: Android
tags: [REST]
---
REST架构是Google I/O 2013年提出
<!--more-->
大致的模型如下图所示
![](/img/14091502.png)

## 应用架构
Activity复制初始化LoaderManager(加载管理器)和内容加载进程，加载管理器接收ContentProvider的变更通知从而通知Adapter修改ListView的内容。
大致分为：
- 显示部分
- 加载后台
- 服务器

### 网络通信
原始的HttpClient开发成本高，且稳定性、安全性、实用性并不高，最高使用先进的网络访问库
- [Volley](https://android.googlesource.com/platform/frameworks/volley) Google官方于2013年I/O大会发布
- [AsyncHttpClient](https://github.com/AsyncHttpClient/async-http-client) SonaType的一个开源库，移植到Android平台需要进行一些定制
- [Retrofit](http://square.github.io/retrofit/) 专注于网络请求和数据解析

### 数据解析
Android默认使用`JsonObject`来解析数据，处理方式比较远(ma)古(fan)，下面的一些开源库可以帮助快速、安全的解析Json数据
- [Gson](https://www.google.com.hk/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0CCsQFjAA&url=http%3a%2f%2fcode%2egoogle%2ecom%2fp%2fgoogle-gson%2f&ei=Vv3sUengEoPJkwXcvYDIDg&usg=AFQjCNGGFFMez8-PfFoEQP93a7eHFY8ssA) Google官方出品，速度快，解析方便
- [Jackson](http://jackson.codehaus.org/) 小有名气，但是导入的包相对多
- [Fastjson](http://code.alibabatech.com/wiki/display/FastJSON/Home-zh) 阿里主导的开源项目，每个属性都必须具备getter/setter方法

### 数据存储
`ContentProvider`原本用于跨进程、夸应用的数据访问，提供了清晰的接口来访问数据库，而且可以使用`CursorLoader`。
`CursorLoader`是目前`Activity`和`Fragment`中异步读取`ContentProvider`的最佳方案。CursorLoader会接到数据变更的通知，可以保证Cursor的数据始终与数据库同步。
