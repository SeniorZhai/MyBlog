title: Activity启动模式
date: 2014-06-18 13:27:47
categories: Android
tags: [Android]
---
针对Activity而言，Android存在一个回退栈维护界面体验。当一个应用程序图标被点击时，应用的Task就会来到前台，并把应用的主Activity压入BackStack的栈顶，并获得焦点，这个Activity称之为根Activity，而BackStack中的Activity可以通过点击回退键弹出栈并销毁，使上一个Activity获得焦点，直到用户返回到Home页，而当BackStack中的Activity都被销毁之后，这个Task就不服存在，这个程序的进程还存在（不在此时销毁）。

## Task的状态
每个个Task都存在一个BackStack，而系统中可以存在多个Task，但是每次只有一个Task获得前台焦点，一般而言，系统允许用户在多个Task中切换，而被置于后台的Task的Activity将被置于Stopped状态。实际上，同一个Task中的Activity，只要不存在于栈顶并且获得前台焦点，那么它就是一个Stopped的状态。
![](https://github.com/zt1991616/blog/raw/master/Image/14061802.png)
## Activity启动模式
根据Activity的不同启动模式，它在BackStack中的状态时不一样的。Activity可以通过AndroidManifest.xml清单文件配置，在<Activity />节点中的android:laucherMode属性设置。它有四个选项：
- standard
- singleTop
- singleTask
- singeInstance
### standard
标准启动模式，也是默认缺省值。standard模式下的Activity会依照启动顺序压入BackStack中，示意图如下：
![](https://github.com/zt1991616/blog/raw/master/Image/14061803.png)
### singleTop
单顶模式，这种模式下，启动一个Activity的时候如果发现BackStack的栈顶已经存在这个Activity了，就不会去重建新的Activity，而是复用这个栈顶已经存在的Activity，避免同一个Activity被重复开启。示意图如下：
![](https://github.com/zt1991616/blog/raw/master/Image/14061804.png)
	singleTop应用的场景很多，一般适用于可以复用而又有多个开启渠道的Activity，避免当一个Activity已经开启并获得焦点后，再次重复开启。比如说Android系统浏览器的书签页面。
### singleTask
开启一个Activity的时候，检查BackStack里面是否有这个Activity的实例存在，如果存在，清空BackStack里这个Activity上所有的其他Activity。
![](https://github.com/zt1991616/blog/raw/master/Image/14061805.png)
### singleInstance
被标记为singleInstance启动模式的Activity，在启动的时候，会开启一个新的BackStack，这个BackStack里只有一个Activity的实例存在，并且使这个BackStack获得焦点。这是一种极端的模式，它会导致整个设备的操作系统里，只存在一个这个Activity实例，不论从何处启动的。
![](https://github.com/zt1991616/blog/raw/master/Image/14061805.png)
singleInstance一般只存在一个适用场景，Android系统的来电页面，多次来电均使用的是一个Activity。


当然，在Android中，除了在AndroidManifest.xml清单文件中配置LauncherMode属性外，还可以在代码中设置启动模式。在组件中，启动一个Activity，需要用到startActivity()方法，其中传递一个Intent，可以使用Intent.setFlags(int flags)来设置新启动的Activity的启动模式，而通过代码设置Activity的启动模式的方式，优先级要高于在AndroidManifest.xml清单文件中的配置。 

Intent.setFlag(int flags)方法传递的一个整形的数据，被Android系统设置为了常量：

- FLAG_ACTIVITY_NEW_TASK：这个标识会使新启动的Activity独立创建一个Task。
- FLAG_ACTIVITY_CLEAR_TOP：这个标识会使新启动的Activity检查是否存在于Task中，如果存在则清除其之上的Activity，使它获得焦点，并不重新实例化一个Activity，一般结合FLAG_ACTIVITY_NEW_TASK一起使用。
- FLAG_ACTIVITY_SINGLE_TOP：等同于在LauncherMode属性设置为singleTop。