title: AVOS Cloud Android开发指南
date: 2014-07-10 13:38:39
categories: Android
tags: [Android]
---
##模块与SDK包
###AVOS Cloud基本存储模块
- avoscloud- 版本号.jar
- android-async-http-1.4.4-fix.jar
- fastjson.jar
- httpmime-4.2.4.jar

###AVOS Cloud 推送模块
- AVOS Cloud 基础存储模块
- avospush- 版本号.jar

###AVOS Cloud 统计模块
- AVOS Cloud 基础存储模块
- avosstatistics- 版本号.jar

###AVOS Cloud SNS 模块
- AVOS Cloud 基础存储模块
- weibo.sdk.android.sso.jar
- qq.sdk.1.6.1.jar

##简介
AVOSCloud提供了一个完整的后端解决方案

##应用程序
在 AVOS Cloud 平台注册后，您创建的每个应用程序都有其自己的应用程序 ID 和 Key, 在您的应用程序中将凭此 ID 和 Key 使用 AVOS Cloud SDK。您的账户可以创建容纳多个应用程序，这是非常方便和有用的。即使您只有一个应用程序，也可以使用不同的版本进行测试和生产。

###对象
AVOS Cloud存储的数据是建立在`AVObject`基础上，每个`AVObject`包含键(key)-值(value)对的JSON兼容的数据。
键必须是字母、数字的字符串，值可以是字符串、数字、布尔值、JSON数组和AVObject对象等。每个`AVObject`有一个类名，你可以用它来区分各种不同的数据。
####保存对象
```java
    AVObject myObj = new AVObject("MyObject");
	myObj.put("value1",123);
    myObj.put("value2",true);
	myObj.put("value3","hello");		
	try{
		    myObj.save();
	} 
    catch (AVException e) {
		    e.getMessage();
		}
    myObj.saveInBackground(new SaveCallback() {
		@Override
		public void done(AVException e) {
			if (e == null) {
				// 保存成功
			} else {
				// 保存失败
			}
		}
	});
```
####检索对象
使用`AVQuery`通过`ObjectID`检索到一个完整的AVObject
```java
	AVQuery<AVObject> query = new AVQuery<AVObject>("MyObject");
	AVObject myObj;
	try {
		myObj = query.get("51c912bee4b012f89e344ae9");
	} catch (AVException e) {
		// e.getMessage();	
	}
	query.getInBackground("51c912bee4b012f89e344ae9", new GetCallback<AVObject>() {
		@Override
		public void done(AVObject obj, AVException e) {
			if (e == null) {
				// 获取成功
			} else {
				// 获取失败
			}
		}
	});
```
####更新对象
获取AVObject对象，然后进行修改值后保存数据
```java
    AVQuery<AVObject> query = new AVQuery<AVObject>("MyObject");
	AVObject myObj;
	try {
		myObj = query.get("51c912bee4b012f89e344ae9");
		myObj.put("value3", "hello world");
	} catch (AVException e) {
		// e.getMessage();
	}
```
####计数器
```java
myObj.increment("value1");
// myObj.increment(key,amount);方法可以递增递减任意幅度的数字
```
####更新后获取最新值
设置`fetchWhenSave`属性为`true`会使更新后，AVObject获得最新值
```java
myObj.setFetchWhenSave(true);
myObj.increment("value1");
myObj.saveInBackground(new SavaCallback(){
    @Override
    public void done(AVException e){
        //
    }
});
```
####删除对象
从服务器删除对象
```java
myObj.deleteInBackground();
// 删除value3字段
myObj.remove("value3");
myObj.saveInBackground();
// 批量删除对象
List<AVObject> objects = ...
AVObject.deleteAll(objects);
```
####关联数据
对象可以与其他对象相联系，就像数据库中的主外键关系一样，数据表A的某一个字段是数据表B的外键，只有表B中存在的数据才插入进A中的字段。
```java
AVObject myWeibo = new AVObject("Post");
myWeibo.put("content", "正文");
AVObject myConment = new AVObject("Comment");
myConment.put("content", "评论");
myConment.put("post",myWeibo);
myConment.saveInBackground();
// 通过objectId来关联已用对象
myComment.put("post",AVObject.createWithoutData("Post", "1zEcyElZ80"));
```
默认情况下，获取一个对象的时候，关联的`AVObject`不会被获取，这些对象的值无法获取，直到调用`fetch`
```java
myConment.getAVObject("post").fetchIfNeededInBackground(new GetCallback<AVObject>() {
	@Override
	public void done(AVObject object, AVException e) {
		String content = object.getString("content");
	}
});
```
使用`AVRelation`来建模多对多关系。
比如一个`User`喜欢很多`Post`，可以用`getRelation`方法保存一个用户喜欢的用户的Post集合。```java
AVUser user = AVUser.getCurrentUser();
AVRelation<AVObject> relation = user.getRelation("likes");
relation.add(post);
user.saveInBackground();
// 从AVRelation中移除一个Post
relation.remove(post)
```
默认情况，处于关系的对象集合不会被下载，可以通过`getQuery`方法返回的`AVQuery`对象，使用它的findInBackground方法来获取Post链表
```java
relation.getQuery().findInBackground(new FindCallback<AVObject>(){
   void done(List<AVObject> result,AVException e){
       if(e == null){
           //
       } else {
           //
       }
   } 
});
// 获取链表的一个子集合，可以添加更多的约束条件到`getQuery`返回`AVQuery`对象
AVQuery<AVObject> query = relation.getQuery();
// 已持有一个post对象，想知道它被哪些User所喜欢，反查询
AVQuery<AVObject> userQuery = AVRelation.reverseQuery("_User","likes",myPost);
userQuery.findInBackground(new FindCallBack<AVObject>(){
   @Override
   public void done(List<AVObject> users,AVException e){
       //
   }
});
```
####数据类型
支持的数据类型有`String`、`Int`、`Boolean`、`AVObject`，同时支持`java.util.Date`、`byte[]`、`JSONObject`、`JSONArray`数据类型。

###查询
####基本查询
先创建一个`AVQuery`对象，然后通过添加不同的条件，使用`findInBackground`方法结合`FindCallback`回调类来查询与条件匹配的AVObject数据，使用`whereEqualTo`方法来添加条件值
```java
AVQuery<AVObject> query = new AVQuery<AVObject>("MyObject");
query.whereEqualTo("value1","value2");
query.findInBackgroud(new FindCallback<AVObject>(){
   public void done(List<AVOject> object,AVException e){
       if (e == null) {
           //
       } else {
           //
       }
   } 
});
```
#####查询条件
- whereNotEqualTo() 不等于
- setLimit() 限制结果的个数
- setSkip() 忽略多少个
- orderByAscending() 升序排列
- orderByDescending() 降序排列
- whereLessThan() 小于
- whereLessThanOrEqualTo() 小于等于
- whereGreaterThan() 大于
- whereGreaterThanOrEqualTo() 大于等于

想查询匹配几个不同值的数据，如：要查询"steve"、"chard"、"jack"三个人的成绩，可以使用`whereContainedIn`方法来实现，排除可以使用`whereNotContainedIn`方法
```java
String[] names = {"steve","chard","jack"};
query.whereContainedIn("playName",Arrays.asList(names));
```
使用`whereMatches`方法可以使用任何正确的正则表达式来检索匹配的值
```java
// 比较name字段的值是以大写字母和数字开头
AVQuery<AVObject> query = new AVQuery<AVObject>("GameScore");
query.whereMatches("name", "^[A-Z]\\d");

query.findInBackground(new FindCallback<AVObject>() {
    public void done(List<AVObject> sauceList, AVException e) {

    }
});
```
查询字符串中包含“XX“内容，可用如下方法：
```java
// 查询playerName字段的值中包含“ste“字的数据
AVQuery query = new AVQuery("GameSauce");
query.whereContains("playerName", "ste");

// 查询playerName字段的值是以“cha“字开头的数据
AVQuery query = new AVQuery("GameSauce");
query.whereStartsWith("playerName", "cha");

// 查询playerName字段的值是以“vj“字结尾的数据
AVQuery query = new AVQuery("GameSauce");
query.whereEndsWith("playerName", "vj");
```
#####数组查询
如果key对应的值是一个数组，可以查询key的数组包含了数字2的所有对象
```java
query.whereEqualTo("arrayKey",2);
```
同样，你可以查询出 Key 的数组同时包含了 2,3 和 4 的所有对象：
```java
//查找出所有arrayKey对应的数组同时包含了数字2,3,4的所有对象。
ArrayList<Integer> numbers = new ArrayList<Integer>();
numbers.add(2);
numbers.add(3);
numbers.add(4);
query.whereContainsAll("arrayKey", numbers);
```
#####字符串的查询
使用 whereStartsWith 方法来限制字符串的值以另一个字符串开头。非常类似 MySQL 的 LIKE 查询，这样的查询会走索引，因此对于大数据集也一样高效：
```java
//查找出所有username以avos开头的用户
AVQuery<AVObject> query = AVQuery.getQuery("_User");
query.whereStartsWith("username", "avos");
```
#####查询对象个数
query使用count替代find可以统计多少个对象满足查询
```java
query.countInBackgroud(new CountCallback(){
   @Ovrride
   public void done (int count,AVException e) {
       if(e == null){
           // count 就是符合查询条件的个数
       }
   }
});
```