title: SQLite
date: 2014-06-23 13:29:42
categories: Android
tags: [Android]
---
创建SQLite数据库推荐继承SQLiteOpenHelper类，然后重写其中的onCreate()方法，在onCreate()方法中，执行数据库创建的SQL语句。而SQLiteOpenHelper不仅仅用于SQLite的创建，也可以对其进行维护，以及获得SQLiteDatabase这个数据库操作对象。
SQLiteOpenHelper提供了两个构造器，用于传递当前上下文对象以及SQLite数据库版本信息，在SQLiteOpenHelper的继承类构造方法中，会调用它：
```java
// context是上下文对象；name是数据库名称；factory是一个允许子类在查询时使用的游标，一般不用传null;version是数据库版本号；errorHandler是一个接口，当数据库错误的时候，执行的补救方法。
SQLiteOpenHelper(Context context,String name,SQLiteDatabase.CursorFactory factory,int version).
SQLiteOpenHelper(Context context,String name,SQLiteDatabase.CursorFactroy factory,int version,DatabaseErrorHandler errorHandler).
```
常用的方法：
- `String getDatabaseName()`:获取数据库名。
- `SQLiteDatabase getReadableDatabase()`:创建或者打开一个可读的数据库对象。
- `SQLiteDatabase getWritableDatabase()`:创建或者打开一个可读/写的数据库对象。
- `abstract void onCreate(SQLiteDatabase db)`:当第一次调用SQLiteOpenHelper的时候执行，之后再次调用将不再执行，一般用于完成数据库初始化的工作。
- `void onUpgrade(SQLiteDatabase db,int oldVersion,int newVersion)`:当数据库版本号发生向上更新时，被执行。
- `void onDowngrade(SQLiteDatabase db,int oldVersion,int newVersion)`当数据库版本号发生向下更新时，被执行。

执行CRUD
　当使用SQLiteOpenHelper的getReadableDatabase()或者getWritableDatabase()方法获取到SQLiteDatabase对象，就可以对这个数据库进行操作了。

对于熟悉SQL语句的开发者而言，其实只需要使用两个方法，即可执行所有CRUD操作，以下方法提供多个重载方法：
- void execSQL()：通过SQL语句执行一条非查询语句。
- Cursor rawQuery()：通过SQL语句执行一条查询语句。