title: ReactNative-Navigator
date: 2016-01-17 23:22:02
categories: ReactNative
tags: [Navigator,路由,切换]
---
<!--more-->
#基本用法
Navigator可以切换不同场景，导航器通过路由来分辨不同的场景。
`renderScene`方法用来指定渲染的场景，`configureScene`属性指定路由对象的配置信息，可以指定进场动画或者手势。

#方法
- `getCurrentRoutes()` 获取当前栈里的路由
- `jumpBack()` 跳回之前的路由，当前场景保留
- `jumpForward()` 跳回之后的路由
- `jumpTo(route)` 跳转到已有的场景并且不卸载
- `push(route)` 跳转到新的场景
- `pop()` 跳转出去并且卸载当前场景
- `replace(route)` 用一个新的路由替换掉当前场景
- `replaceAtIndex(rote,index)` 替换掉指定序列的场景
- `replacePrevious(route)` 替换掉之前的场景
- `immediatelyResetRouteStack(routeStack)`  用新的路由重置路由栈
- `popToRoute(route)` pop到路由指定的场景，其他的场景将被卸载
- `popToTp()` pop到栈中单第一个场景，卸载掉所有的其他场景
#属性
- `configureScene` 可选函数，用来指定场景动画和手势
- `initialRoute` 指定启动时加载的路由
- `initialRouteStack` 指定一个路由集合来初始化
- `navigatorBar` 可选参数，提供一个场景切换时保持的导航栏
- `navigator` 可选参数，提供父类导航获取导航器对象
- `renderScene` 必选参数，用来指定路由渲染的场景
- `sceneStyle` 指定每个场景的容器上的样式
