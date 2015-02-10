title: LitePal的使用
date: 2015-02-02 11:20:19
categories: Android
tags: [数据库,sqlite]
---
LitePal是一款开源的Android数据库框架，采用了ORM对象关系映射的模式，将常用的数据库功能进行了封装。
<!--more-->
##基本用法
1. 引入jar包
 	可以在[这里](https://github.com/LitePalFramework/LitePal#latest-downloads)下载LitePal的最新版本
 	也可以在github上下载LitePal的源码，使用Library的方式导入Eclipse中
2. 配置litepal.xml
	在`assets`目录下建立litepal.xml文件
	```xml
	<?xml version="1.0" encoding="utf-8"?>
	<litepal>
		<dbname value="demo"></dbname>
		<version value="1"></version>
		<list>
		</list>
	</litepal>
	```
	`dbname`用于设定数据库的名字，`version`用于设定数据库版本号，`list`用于设定所有的映射模型
3. 配置LitePalApplication
	在`AndroidManifest.xml`中配置一个`LitePalApplication`
	```xml
	<manifest>
		<application
			name=".LitePalApplication"
			...
			>
		...
		</application>
	</manifest>	
	```
	如果使用了自定义的Application继承LitePalApplication即可

###建表
根据对象关系映射模式的概念，每一张表应该对应一个模型，比如对应的News模型
```java
public class News{
	private int id;			// 可以不写，LitePalace会自定生成
	private String title;
	private String content;
	private Date publishDate;
	private int commentCount;
	// getter、setter
}
```
映射的数据类型一共有8种:`int`、`short`、`long`、`float`、`double`、`boolean`、`String`和`Date`
使用LitePal只有声明private的字段才会被映射到数据表中，如果不想映射的话，修饰符设置为`public`、`protected`、`default`就可以了
建立后再配置到映射表中，编辑asset目录下的litepal.xml的文件，在`<list>`标签下加入News模型类的声明
```xml
<?xml version="1.0" encoding="utf-8"?>
	<litepal>
		<dbname value="demo"></dbname>
		<version value="1"></version>
		<list>
			<mapping class="com.example.databasetest.model.News"></mapping>
		</list>
</litepal>
```
获取SQLiteDatabase的实例
```java
SQLiteDatabase db = Connector.getDatabase();
```

###升级表
增加新的表或者改变表结构，只需要需要对应model的属性，在`litepal.xml`中进行配置，并对`version`进行加1处理

##中级用法
###建立表关联
1. 一对一关系
比如相应`News`每一条新闻都有一段`introduction`简介
可以在introduction中设置一个news_id的外键列，存放具体新闻的id
2. 多对一关系
比如一个`News`对应多个`Comment`评论
在`Comment`中设置`new_id`列，存放具体New的ID，多个comment就对应了同一个New
3. 多对多
比如`News`可以有很多`Category`种类，`Category`对应很多`News`
可以新建一个中间表来存放news和category的关系
category_news表设置`news_id`和`category_id`两列，分别是news表的外键和category表的外键

###使用LitePal建立关联表
新建`Introduction`和`Category`两个类
```java
public class Introduction {
	private int id;
	private String guidie;
	private String digest;
	// getter、setter
}

public class Category {
	private int id;
	private int name;
	// getter、setter
}
```
在`News`中添加`Introduction`的引用
```java
public class News {
	...
	private Introduction introdution;
	...
}
```
因为`Comment`和`News`是多对一的关系
```java
// news对应多个comment
public class News {
	...
	private List<Comment> commentList = new ArrayList<Comment>();
}
// comment 对应一个news
public class Comment {
	...
	private News news;
	...
}
```
对于`news`和`category`
```java
public class News {
	...
	private List<Category> categoryList = new ArrayList<Category>();
	...
}

public class Category {
	...
	private List<News> newsList = new ArrayList<News>();
	...
}
```
###存储
要进行`CRUD`操作所有实体类都需要继承自`DataSupport`类
之后实体类就可以进行`CRUD`操作了
```java
News news = new News();
news.setTitle("标题");
news.setContent("内容");
news.setPubllishDate(new Date);
news.save();	// 返回布尔值，是否存储成功
```
也可以使用`saveThrows()`方法，抛异常来捕捉存储失败的异常
其中`LitePal`已经默默地帮我们设置了id
`Comment`和`News`是多对一的关系
```java
Comment comment1 = new Comment();  
comment1.setContent("好评！");  
comment1.setPublishDate(new Date());  
comment1.save();  

Comment comment2 = new Comment();  
comment2.setContent("赞一个");  
comment2.setPublishDate(new Date());  
comment2.save();  

News news = new News();  
news.getCommentList().add(comment1);  
news.getCommentList().add(comment2);  
news.setTitle("第二条新闻标题");  
news.setContent("第二条新闻内容");  
news.setPublishDate(new Date());  
news.setCommentCount(news.getCommentList().size());  
news.save();  
```
这样多对一的关系就被建立，而且`Comment`中的`news_id`已经被默默赋值
如果有一个`List<News> newsList`，可以使用`DataSupport.saveAll(newsList)`来存储集合数据
###修改数据
比如把`news`表中id为2的记录的标题修改
```java
ContentValues = new ContentValues();
values.put('title',"title修改");
DataSupport.update(News.class,values,2);
```
修改多条数据
```java
ContentValues values = new ContentValues();  
values.put("title", "今日iPhone6 Plus发布");  
DataSupport.updateAll(News.class, values, "title = ?", "今日iPhone6发布");
```
使用约束条件可以限定特定的数据，`?`为占位符，用后面的String数据代入
比如：
```java
ContentValues values = new ContentValues();  
values.put("title", "今日iPhone6 Plus发布");  
DataSupport.updateAll(News.class, values, "title = ? and commentcount > ?", "今日iPhone6发布", "0");
```
如果全部修改可以不使用约束
```java
ContentValues values = new ContentValues();  
values.put("title", "今日iPhone6 Plus发布");  
DataSupport.updateAll(News.class, values);  
```
使用`ContentValues`对象并不是那么友好，也可以直接使用实体类
```java
News updateNews = new News();
updateNews.setTtitle("新标题");
updateNews.update(2);
//...
News updateNews = new News();  
updateNews.setTitle("今日iPhone6发布");  
updateNews.updateAll("title = ? and commentcount > ?", "今日iPhone6发布", "0");  
```
如果要修改某列数据为默认值，使用`setToDefault()`方法

###删除
```java
DataSupport.delete(News.class,2);	// 删除news表中id为2的记录，news_id为2的记录为外键的数据也会删除，返回值为被输出的记录数
DataSupport.deleteAll(News.class,"title = ? and commentcount = ? ","title","0"); 	// 批量删除
DataSupport.deleteAll(News.class);	// 删除所有
```
###查询
```java
News news = DataSupport.find(News.class,1);	// 查询id为1的记录
News first = DataSupport.findFirst(News.class); // 查询第一条数据
News last = DataSupport.findLast(News.class);	// 查询最后一条数据
List<News> newsList = DataSupport.findAll(News.class,1,3,5,7);	// 查询id为1,3,5,7的数据
long[]ids = new long[]{1,3,5,7};
List<News> newsList2 = DataSupport.findAll(News.class,ids);
List<News> newsList3 = DataSupport.findAll(News.class);	// 所有数据
```
####连缀查询
根据指定查询条件查询
```java
List<News> newsList = DataSupport.where("commentcount > ?","0").find(News.class);
List<News> newsList = DataSupport.select("title", "content")  
        .where("commentcount > ?", "0").find(News.class);  	// 只要title和content两列数据
List<News> newsList = DataSupport.select("title", "content")  
        .where("commentcount > ?", "0")  
        .order("publishdate desc").find(News.class);  	// asc正序排序、desc倒序排序
List<News> newsList = DataSupport.select("title", "content")  
        .where("commentcount > ?", "0")  
        .order("publishdate desc").limit(10).find(News.class);  // 只查询10条数据
List<News> newsList = DataSupport.select("title", "content")  
        .where("commentcount > ?", "0")  
        .order("publishdate desc").limit(10).offset(10)  
        .find(News.class);  									// 查询11-20
```
###激进查询
上述的查询无法查询关联表的数据，LitePal默认的模式就是懒查询，如果要一次性将关联表中的数据也一起查询出来，LitePal也支持激进查询的方式。
```java
News news = DataSupport.find(News.class,1,true);		// true参数表示使用激进查询，关联的数据也会一起查出来
List<Comment> commentList = news.getCommentList();
```
使用懒加载也可以查询出关联数据，比如在News类中增加代码
```java
public class News extends DataSupport {
	...
	public List<Comment> getComments() {
		return DataSupport.where(news_id=?),String.valueOf(id)).find(Comment.class);
	}
}
```
###原生查询
LitaPal也可以使用原生的查询(SQL语句)
```java
Cursor cursor = DataSupport.findBySQL("select * from news where commentcount>?","0");
```
##聚合函数
LitePal中提供了count()、sum()、average()、max()、min()这五种聚合函数

- count()
	count()主要用于统计行数
	```java
	int result = DataSupport.count(News.class);
	int result = DataSupport.where("commnetcount = ?","0").count(News.class);
	```	
- sum()
	sum()用于对结果进行求和
	```java
	// 第一个参数表示表，第二个参数是列名，第三个参数用于指定结果的类型
	int result = DataSupport.sum(News.class,"commentcount",int.class);
	// 表示所有的评论数
	```
- average()
	average()用于统计平均数
	```java
	double result = DataSupport.average(News.class,"commentcount");
	```
- max()
	求出列中最大的数值
	```java
	int result = DataSupport.max(News.class,"commentcount",int.class);
	```
- min()
	求出列中最小的数值
	```java
	int result = DataSupport.min(News.class,"commentcount",int.class);
	```