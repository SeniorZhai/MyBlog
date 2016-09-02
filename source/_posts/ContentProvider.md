title: ContentProvider
date: 2014-06-25 13:30:54
categories: Android
tags: [Android]
---
## 概述
ContentProvider是Android应用对外开放的数据接口，只要符合它所定义的Uri格式的请求，均可以正常访问执行操作。其他的Android应用可以使用ContentResolver对象通过与ContentProvider同名的方法请求执行，被执行的就是ContentProvider中的同名的方法。所以ContentProvider很多对外可以访问的方法，在ContentResolver中均有同名的方法，是一一对应的，如图：

![](https://github.com/zt1991616/blog/raw/master/Image/14062501.png)

## Uri
在Android中，Uri是一种比较常见的资源访问方式，而对ContentProvider而言，Uri也是有固定格式的：
<srandard_prefix>://<authority>/<data_path>/<id>
- <srandard_prefix>：ContentProvider的srandard_prefix始终是content://
- <authority>：ContentProvider的名称
- <data_path>：请求的数据类型
- <id>：指定请求的特定数据

## ContentProvider
　ContentProvider也是Android应用的四大组件之一，所以也需要在AndroidManifest.xml文件中进行配置。而且某个应用程序通过ContentProvider暴露了自己的数据操作接口，那么不管该应用程序是否启动，其他应用程序都可以通过这个接口来操作它的内部数据。
　Android附带了许多有用的ContentProvider，但是本篇博客不会涉及到这些内容的，以后有时间会再讲解。Android附带的ContentProvider包括：
- Browser：存储如浏览器的信息。
- CallLog：存储通话记录等信息。
- Contacts：存储联系人等信息。
- MediaStore：存储媒体文件的信息。
- Settings：存储设备的设置和首选项信息。
在Android中，如果要创建自己的内容提供者的时候，需要扩展抽象类ContentProvider，并重写其中定义的各种方法。然后在AndroidManifest.xml文件中注册该ContentProvider即可。

ContentProvider是内容提供者，实现Android应用之间的数据交互，对于数据操作，无非也就是CRUD而已。下面是ContentProvider必须要实现的几个方法：
- onCreate()：初始化提供者。
- query(Uri, String[], String, String[], String)：查询数据，返回一个数据Cursor对象。
- insert(Uri, ContentValues)：插入一条数据。
- update(Uri, ContentValues, String, String[])：根据条件更新数据。
- delete(Uri, String, String[])：根据条件删除数据。
- getType(Uri) 返回MIME类型对应内容的URI。

除了onCreate()和getType()方法外，其他的均为CRUD操作，这些方法中，Uri参数为与ContentProvider匹配的请求Uri，剩下的参数可以参见SQLite的CRUD操作，基本一致。
还有两个方法：call()和bulkInsert()方法，使用call，理论上可以在ContentResolver中执行ContentProvider暴露出来的任何方法，而bulkInsert()方法用于插入多条数据。

在ContentProvider的CRUD操作，均会传递一个Uri对象，通过这个对象来匹配对应的请求，那么如何确定一个Uri执行哪项操作呢？需要用到一个UriMatcher对象，这个对象用来帮助内容提供者匹配Uri，它所提供的方法非常简单，仅有两个：
- void addURI(String authoity,String path,int code):添加一个Uri匹配项，authtity为AndroidManifest.xml中注册的ContentProvider的authority属性；path为一个路径，可以设置通配符，# 表示任意数字，*表示任意字符；code为自定义的一个Uri代码
- int match(Uri uri):匹配传递的Uri，返回addUri()传递的Code参数。

在创建好一个ContentProvider之后，还需要在AndroidManifest.xml文件中对ContentProvider进行配置，使用一个<provider.../>节点，一般只需要设置两个属性即可访问，一些额外的属性就是为了设置访问权限而存在的
- android:name:provide的响应类
- android:authorities:Provider的唯一标识，用于Uri匹配，一般为ContentProvider类的全名

## ContentResolver
ContentResolver，内容访问者。可以通过ContentResolver来操作ContentProvider所暴露处理的接口。一般使用Content.getContentResolver()方法获取ContentResolver对象，上面已经提到ContentResolver的很多方法与ContentProvider————对应，所以它存在insert、query、update、delete等方法。

### getType()中的MIME
MIME类型就是设定某种扩展名的文件用一种应用程序来打开的方式类型。在ContentProvider中的getType方法，返回的就是一个MIME的字符串。如果支持需要使用ContentProvider来访问数据，getType()完全可以返回一个Null，并不影响效果，但是覆盖ContentProvider的getType方法对于用new Intent(String action,Uri uri)方法启动activity是很重要的，如果它返回的MIME type和activity在<intent filter>中定义的data的MIME type不一致，将造成activity无法启动。
getType返回的字符串，如果URI针对的是单条数据，则返回的字符串以`vnd.android.cursor.item/`开头；如果是多条数据，则以`vnd.android.cursor.dir/`开头。

### 访问权限
对于ContentProvider暴露出来的数据，应该是储存在自己应用内存中的数据，对于一些储存在外部储存器上发数据，并不能限制访问权限，使用ContentProvider就没有意义了。对于ContentProvider而言，有很多权限控制，可以AndroidManifest.xml文件中对<provider>节点的属性进行配置，一般使用如下一些属性设置：
- android:grantUriPermssions:临时许可标志。
- android:permission:Provider读写权限。
- android:readPermission:Provider的读权限。
- android:writePermission:Provider的写权限。
- android:enabled:标记允许系统启动Provider。
- android:exported:标记允许其他应用程序使用这个Provider。
- android:multiProcess:标记允许系统启动Provider相同的进程中调用客户端。