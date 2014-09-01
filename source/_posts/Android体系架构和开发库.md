title: Android体系架构和开发库
date: 2014-05-09 13:23:31
categories: Android
tags: [Android]
---
Android的体系架构鼓励组建重用，允许开发者发布共享Activity、Service并且访问其他应用程序的数据，还可以根据开发者制定的安全限制进行管理。

##剖析Android体系架构

第一个列表将向开发者展示应用服务，这些服务是Android的架构基石。你也可以称它为所有的Android应用程序的支柱框架，为所有开发应用提供支持。
	- Activity Manager:控制界面的生命周期，包括管理活动(Activity)栈
	- Views:Views为应用程序构建用户界面。
	- Notification Manager:提供一贯的非侵入式的机制来通知用户。
	- Content Providers:让开发者在不同应用之间共享数据。
	- Resource Manager:支持非代码资源，如字符串和图形被外部化。

#Android库

Android也提供了大量的API开发应用程序。
- android.util:核心工具包种包含底层类，字符串格式化和XML解析工具、底层类
- android.os:操作系统包提供了访问基本的操作系统服务，如消息传递、进程通信、时钟功能和调试。
- android.graphics:图形API提供了支持画布、颜色和回执图元的低级别的图形类并且支持绘制画布。
- android.text:用于显示和歇息文本的处理工具
- android.database:在数据库处理游标时提供底层类支持
- android.content:content API管理数据访问，提供服务来管理资源、内容提供者（content provider）和包
- android.view:视图是核心的用户接口类，所有用户界面元素使用的是一系列视图，以构成用户交互的组件
- android.widget:内置在View包内，小部件类是”这里是我们前面创建的“用户界面元素，可以在自己的应用程序中使用
- com.google.android.maps:高级API，它提供了访问本地地图控件，可以在自己的应用程序中使用，包括MapView的控制、用于标注和控制您的前途地图的叠加以及MapController类
- android.app:一个高层次的包，允许访问应用程序模型。该应用程序包括Activity和Service的API等是Android应用程序的基础
- android.provider:方便开发者访问标准的内容提供者（比如联系人数据库），provider包提供类给开发者访问标准的数据库
- android.telephony:telephony API让开发者直接接触电话底层，开发者可以打电话、接电话、显示通话记录、通话状态和短消息
- android.webkit:WebKit的软件包功能的API与基于Web的内容的工作，其中包括一个WebView的控件在您的活动中嵌入浏览器和cookie管理器

除了在Android API，Android栈还包括一组的`C/C++`库，可通过应用程序框架发布出来。
- OpenGl:用于支持基于OpenGL ES1.0 API、3D图像库
- FreeType:支持位图和矢量字体渲染
- SGL:用于提供2D图形和矢量字体渲染
- ibc:标准C库，为基于Linux的嵌入式设备进行了优化
- SQLite:用于存储应用程序数据的轻量级的关系数据库引擎
- SSL:支持使用安全套接字层加密协议进行安全互联网通信

##高级Android开发库

- android.location:基于位置的服务的API，使应用程序访问设备的当前物理位置，基于位置的服务提供通的访问使用任何位置固定的硬件或技术设备上可用的位置信息。
- android.media:没提API提供了用于播放音频和视频没提文件，包括流媒体和录制的支持。
- android.opengl:Android提供使用的OpenGLass ES API，你可以用它来创建动态3D用户界面为你的应用程序的强大的3D渲染引擎
- android.hardware:如果有可能，硬件API公开的传感器硬件，包括摄像头、加速计和指南针传感器
- android.bluetooth,android.net.wifi,android.telephony:Android也提供了硬件平台，包括蓝牙、Wi-Fi和电话硬件的低级别的访问
