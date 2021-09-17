title: Contour
date: 2020-11-14 15:05:09
categories: Android
tags: [layout, declarative]
---

<!--more-->

## Transition
Transition内部使用了属性动画实现，是一种属性动画的封装。Transition主要负责两个方面，一是保存开始和结束场景的状态。二是两种状态之间创建动画。

### Scene 
Scene 场景过渡动画就是实现View从一个状态变化到另一个状态，Scene就代表一个场景，内部保存一个完整的视图结构

Scene(ViewGroup sceneRoot)
Scene(ViewGroup sceneRoot, View layout)

