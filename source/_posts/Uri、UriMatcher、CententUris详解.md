title: Uri、UriMatcher、CententUris详解
date: 2014-07-03 13:33:21
categories: Android
tags: [Android]
---

1. Uri
通用资源标志符(Universal Resource Ident，简称"URI")
Uri代表要操作的数据，Android上可用的每种资源——图片、视频等资源都可以用Uri表示
URI一般分为三个部分组成：`访问资源的命名机制`，`存放资源的主机名`,`资源自身的名字`，由路径表示

Android的Uri由以下三部分组成："content://"、数据的路径、标示ID(可选)
如：
- 所有联系人的Uri：content://contacts/people
- 某个联系人的Uri：content://contacts/people/5
- 所有图片Uri:content://media/external
- 某个图片的Uri:content://media/external/images/media/4

2. UriMatcher
UriMatcher类主要用于匹配Uri
使用方法如下，初始化：
```java
UriMathcer matcher = new UriMatcher(UriMatcher.NO_MATCH);
```
第二步注册需要的Ur:
```java
matcher.addURI("com.zoe.blog","people",PEOPLE);
matcher.addURI("com.zoe.blog", "person/# ", PEOPLE_ID); 
```
第三部，与已经注册的Uri进行匹配:
```java
Uri uri = Uri.parse("content://" + "com.zoe.blog" + "/people");
int match = matcher.match(uri);
switch(match)
{
    case PEOPLE:
        return "vnd.android.cursor.dir/people";
    case PEOPLE_ID:
        return "vnd.android.cursor.item/people";
    default:
        return null;
}
```
match方法匹配后会返回一个匹配码Code，即在使用注册方法addURI时传入的第三个参数。
- 常量`UriMatcher.NO_MATCH`表示不匹配任何路径的返回码
- `# 号`为通配符
- `*号`为任意字符
3. CententUris
ContentUris类用于获取Uri路径后面的ID部分
```
Uri uri = Uri.parse("content://com.zoe.demo/people");
Uri resultUri = ContentUris.withAppendedId(uri,10);
//resultUri为：content://com.zoe.demo/people/10
ling id = ContentUris.parseId(uri);
//id = 10
```