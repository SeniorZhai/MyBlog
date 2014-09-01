title: CursorLoader
date: 2014-08-07 13:48:36
categories: Android
tags: [Android]
---
从`ContentProvider`查询数据比较耗时，在UI线程中查询可能会ANR，通过`CursorLoader`来实现，异步查询数据。
##使用
`CursorLoader`依靠`ContentProvider`在后台执行一个异步的查询操作，并且返回数据给调用它的Activity或者Fragment。
1. 定义使用CursorLoader的Activity
    必须实现`LoaderCallbacks`接口，`CursorLoader`会触发这些回调方法。
```java
public class OtherFragment extends FragmentActivity implements LoaderManager.LoaderCallback<Cursor>{
    ...
}
```
2. 初始化查询
    为了初始化查询，需要执行`LoaderManager.initLoader()`初始化后台任务。可以在`onCreate()`或者`onCreateView()`中触发这个方法。
```java
    getLoaderManager().initLoader(1,null,this); // 第一个参数为标志 第二个为可选的参数供Loader构造 第三个为Loader接口
```
注：`getLoaderManager()`只能在Fragment类中调用，在FragmentActicity中应该使用`getSupportLoaderManager()`
3. 开始查询
一旦初始化完成，它会回调`onCreateLoader()`方法。为了启动查询任务，在这个方法需要返回`CursorLoader`
```java
@Override
public Loader<Cursor> onCreateLoader(int loaderId,Bundle bundle){
    switch(loaderID) {
        case URL_LOADER:    // 设置的标示符
            return cursorLoader;
            break;
        default:
            return null;
    }
}
```
一旦获取查询任务的Loader对象，就会开始在后台查询任务，当查询完成之后，就会执行`onLoadFinished()`回调函数。
4. 处理查询
`onLoadFinished()`参数之一是`Cursor`，包含了查询的数据
```java
@Override
public void onLoadFinished(Loader<Cursor> loader, Cursor data) {
    // 通知Adapter梗概数据
    mAdapter.changeCursor(data);
}
```
在Cursor失效时，CursorLoader会被重置，通常会发生在Cursor相关的数据改变的时候，在重新执行查询操作之前，系统会执行`onLoaderReset()`方法。在这个方法中，应该删除当前Cursor上的所有数据，避免发生内存泄露。
```java
@Override
public void onLoaderReset(Loader<Cursor> loader) {
    //
    mAdapter.changeCursor(null);
}
```