title: 在Android上使用NoSQL数据库
date: 2015-01-08 10:06:02
categories: Android
tags: [数据库,NoSQL,轻量级]
---
NoSQL泛指非关系数据库，随着web2.0网站的兴起迅速发展。
<!--more-->
相对于SQLite数据库，NoSQL无意更加快速、更加轻量级、存取数据方便，不需要设计多张表。

##SnappyDB
[SnappyDB](https://github.com/nhachicha/SnappyDB)是众多NoSQL数据库的一员，Android上非常流行的NoSQL数据库，可以保存任何基本数据类型和序列化(Serializable)的数据及其数组
SnappyDB在保存和读取序列对象的时候，使用的是[Kryo](https://github.com/EsotericSoftware/kryo)库，也Java内置序列化更快。更大的优势是，你并不要为数据去显式的去实现Serializable接口。这就意味着你以前的代码完全不要做任何改动。
###创建数据库
	DB snappydb = DBFactory.open(context);	// 默认名的数据库
	DB snappydb = DBFactory.open(context,"books");	// 指定名字的数据库
SnappyDB使用内部存储空间，路径为:`/data/data/com.snappydb/files/mydatabase`
也可以使用参数指定存储位置
```java
DB snappyDB = new SnappyDB.Builder(context)
					.directory(Environment.getExternalStrongeDirectory().getAbsolutePath())
					.name("books")
					.build();
```

###关闭数据库
	snappydb.close();

###销毁数据库
	snappydb.destroy();

###插入基本数据
- short `snappyDB.putShort("myshort",(short)32768);`
- int `snappyDB.putInt("myint",Integer.MAX_VALUE);`
- long `snappyDB.putLong("mylong",Long.MAX_VALUE);`
- double `snappyDB.putDouble("my_double",Double.MAX_VALUE);`
- float `snappyDB.putFloat("my_float",10.30f);`
- boolean `snappyDB.putBoolean("my_bollean",true);`
- String `snappyDB.put("my_string","string");`

###读取基本数据
- short `short myshort = snappyDB.getShort("myshort");`
- int `int myInt = snappyDB.getInt("myint");`
- long `long myLong = snappyDB.getLong("mylong");`
- double `double myDouble = snappyDB.getDouble("my_double");`
- float `float myFloat = snappyDB.getFloat("my_float");`
- boolean `boolean myBool = snappyDB.getBoolean("my_bollean");`
- String `String myString = snappyDB.get("my_string");`

###插入序列化对象
	AtomicInteger objAtomicInt = new AtomicInteger (42);
	snappyDB.put("atomic integer", objAtomicInt);

###读取序列化对象
	AtomicInteger myObject = snappyDB.get("atomic integer", AtomicInteger.class);

###插入对象
	MyPojo pojo = new MyPojo ();
	snappyDB.put("my_pojo", pojo);

###读取对象
	MyPojo myObject = snappyDB.getObject("non_serializable", MyPojo.class);

###插入数组
	Number[] array = {new AtomicInteger (42), new BigDecimal("10E8"), Double.valueOf(Math.PI)};
	snappyDB.put("array", array);

###读取数组
	Number [] numbers = snappyDB.getObjectArray("array", Number.class);

。。。待续
