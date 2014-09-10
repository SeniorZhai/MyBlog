title: Gson解析Json
date: 2014-09-10 15:18:50
categories: Android
tags: [Gson,Json]
---
[Gson](https://code.google.com/p/google-gson/)是Google提供的一个很棒的json解析库。
<!--more-->
##解析单个对象
比如有个一个Person类
```java
public class Person{
	public int age;
	public String name;
}
```
解析时
```java
String json = "..."; //json字符串
Person person = new Gson().fromJson(json,Person.class);
```
如果Person中还有一个Date的birth属性
```
public class Person{
	public int age;
	public String name;
	public Date birth;
}
```
解析时设定格式
```java
String json = "..."; 
GsonBuilder gsonBuilder = new GsonBuilder();
gsonBuilder.setDateFormat("yyyy-MM-dd HH:mm:ss");
Gson gson = gsonBuilder.create();
Person person = gson.fromJson(json,Person.class);
```
##命名
gson默认需要类的属性名与JSON对应，但也可以通过`@SerializeName`来修改
```java
public class Feed {
	@SerializeName("birth");
	public Date birthDay;
}
```
##对象的嵌套
比如返回如下的数据
```json
{
	"id":100,
	"name":"zoe"
	"sub":{
		"id":200,
		"name":"zhai"
	}
}
```
对应的对象应该是`
```java
public class Feed {
	public int id;
	public String name;
	public Sub sub;

	public class Sub{
		public int id;
		public String name;
	}
}
```
##对象数组
形如下面的对象数组
```
[{
	"id":100,
	"name":"name100"
},
{
	"id":101,
	"name":"name101"
} 
//...
]
```
解析方法有两种：
1. 解析成数组
```java
Sub[] subs = new Gson().fromJson(json,Sub[].class);
//转List
List<Sub> subs = Arrays.asList(subs);
```
2. 解析成List
```java
Type listType = new TypeToken<ArrayList<Sub>>(){}.getType();
ArrayList<Sub> subs = new Gson().fromJson(json,listType);
```