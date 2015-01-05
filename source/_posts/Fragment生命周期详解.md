title: Fragment生命周期详解
date: 2015-01-05 10:25:15
categories: Android
tags: [Fragment,生命周期]
---
<!--more-->
![](/img/15010501.png)
- onAttach() 关联Activity时调用
- onCreate() 创建Fragment时调用，在这里必须初始化Fragment的基础组件
- onCreateView() Fragment要绘制自己的界面时调用，这个方法必须返回Fragment的layout，也可以返回null(表示没有界面)
- onActivityCreated() 当Activity对象完成自己的onCreate方法时调用
- onStart() Fragment的UI可见时调用
- onResume() Fragment的UI可交互时调用
- onPause() Fragment 可见但不可交互时调用
- onStop() Fragment 完全不可见时调用
- onDestroyView() Fragment 移除视图时调用
- onDestroy() 清理View资源时调用
- onDetach() 失去Activity关联时调用
![](/img/15010502.png)

- 切换到Fragment(第一次)
	+ onAttach
	+ onCreate
	+ onCreateView
	+ onActivityCreated
	+ onStart
	+ onResume

- 屏幕熄灭
	+ onPause
	+ onSaveInstanceState
	+ onStop

- 屏幕解锁
	+ onStart
	+ onResume

- 切换到其他Fragment
	+ onPause
	+ onStop
	+ onDestroyView

- 切换回本身
	+ onCreateView
	+ onActivityCreated
	+ onStart
	+ onResume 

- 回到桌面
	+ onPause
	+ onSaveInstanceState
	+ onStop

- 回到应用 
	+ onStart
	+ onResume

- 退出应用
	+ onPause
	+ onStop
	+ onDestroyView
	+ onDestroy
	+ onDetach