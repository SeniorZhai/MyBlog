title: Room tips
date: 2018-12-03 11:48:50
categories:
tags:
---

<!--more-->
避免接受到不相关的LiveData回调
首先我们要知道Room的实现机制
1. 使用`triggers`监听到了表的增删改查
2. Room创建了一个[InvalidationTracker](https://developer.android.com/reference/android/arch/persistence/room/InvalidationTracker)观察表是否发生改变
3. 当[InvalidationTracker.Observer#onInvalidated](https://developer.android.com/reference/android/arch/persistence/room/InvalidationTracker.Observer.html#onInvalidated%28java.util.Set%3Cjava.lang.String%3E%29)调用，通知Observer重新查询

Room只知道你的表发送了变化，但是不知道具体发生了什么变化，而且并不知道这些变化观察者是否关心。Room本身并不保存数据所以它也无法判断，观察者要的数据要否变化。

所以在实际使用中你需要自己过滤掉不需要的通知

```java
@Dao
abstrat class UserDao : BaseDao<User>() {
    @Query("SELECT * FROM Users WHERE userid = :id")
    protected abstract fun getUserById(id : String) : Flowable<User>

    fun getDistinctUserBy
}
```

