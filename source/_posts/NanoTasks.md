title: NanoTasks
date: 2015-02-27 11:25:57
categories: Android
tags: [异步,Async]
---
[NanoTasks](https://github.com/fabiendevos/nanotasks)是一款轻量级的Android异步任务工具类，是`AsyncTask`的替代品。
<!--more-->
#导入依赖
```Gradle
compile 'com.fabiendevos:nanotasks:1.0.0'
```
#使用
```java
Tasks.executeInBackground(context, new BackgroundWork<Data>() {
    @Override
    public Data doInBackground() throws Exception {
        return fetchData(); // 费时操作
    }
}, new Completion<Data>() {
    @Override
    public void onSuccess(Context context, Data result) {
        display(result);	// 成功：显示结果
    }
    @Override
    public void onError(Context context, Exception e) {
        showError(e);		// 失败：处理错误
    }
});
```
