title: Android Sqlite Tips
date: 2014-07-04 13:35:49
categories: Android
tags: [Android]
---
1. Add column
```sql
ALTER TABLE {tableName} ADD COLUMN COLNew {type};
```
2.Delete column,Rename column
sqlite3不允许指直接delete，rename只能进行以下操作
```sql
create table temp(id integer PRIMARY KEY,code varchar(255));
insert into temp(id,code) select id ,code from t;
```
3.Rename table
```sql
alter table foo rename to bar
```
4.Copy data from one sqlite file to another
```sql
attach 'database2file' as db2;
insert into TANLENAME select * from db2.TANLENAME;
```
5.导出sql
```sql
sqlite3. data.db
>.output dd.sql
>.dump
```
6.导入
```sql
sqlite3 mydb.db
>.read dd.sql
```
7.释放空间
```sql
vacuum
```
## 一些SQLite命令
- database 列出数据库文件名
- tables ?PATTERN? 列出?PATTERN?匹配的表名
- import FILE TABLE 将文件中的数据导入表中
- dump ?TABLE? 生成形成数据表的SQL脚本
- output FILENAME 将输出导入到指定的文件中
- output stdout 将输出打印到屏幕
- mode MODE ?TABLE? 设置数据输出模式(csv,html,tcl...)
- nullvalue STRING 用指定的串代替输出的NULL串
- read FILENAME 指定指定文件中的SQL语句
- schema ?STRING? 打印创建数据库表的SQL语句
- separator STRING 用指定的字符串代替字段分隔符
- show 打印所有SQLite环境变量的设置
- quite 推出命令行接口