title: Loader机制
date: 2014-08-04 13:46:59
categories: Android
tags: [Android]
---
- LoaderManager
	一个与Activity和Fragment有关联的抽象类，用于管理一个或多个Loader实例。每个Activity或Fragment只能有一个LoaderManager。
- LoaderManager.LoaderCallbacks
	提供了客户端的一个callback接口，用于和LoaderManager进行交互。
- AsyncTaskLoader
	一个抽象Loader，提供了一个AsyncTask进行工作
- CursorLoader
	AsyncTaskLoader的子类，用于向ContentResover请求，返回一个Cursor。

## 使用LoaderManagerCallbacks
LoaderManager.LoaderCallbacks是callback接口，包含三个方法：
- onCreateLoader() --- 实例化和返回一个新创建的给定ID的loader
- onLoadFinished() --- 当一个创建好的loader完成了load,调用此函数
- onLoaderReset() --- 当一个创建好的loader要被reset时，调用此函数

