title: NSFile常用方法
date: 2014-04-08 13:05:07
categories: iOS
tags: [iOS]
---
1、常见的NSFileManager文件方法

-(NSData *)contentsAtPath:path　　//从一个文件读取数据

-(BOOL)createFileAtPath: path contents:(NSData *)data attributes:attr　　//向一个文件写入数据

-(BOOL)removeItemAtPath:path error:err　　//删除一个文件

-(BOOL)moveItemAtPath：from toPath:to error:err　　//重命名或者移动一个文件（to不能是已存在的）

-(BOOL)copyItemAtPath:from toPath:to error:err　　//复制文件（to不能是已存在的）

-(BOOL)contentsEqualAtPath:path andPath:path2　　//比较两个文件的内容

-(BOOL)fileExistAtPath:path　　//测试文件是否存在

-(BOOL)isReadableFileAtPath:path　　//测试文件是否存在，并且是否能执行读操作　　

-(BOOL)isWriteableFileAtPath:path　　//测试文件是否存在，并且是否能执行写操作　　

-(NSDictionary *)attributesOfItemAtPath:path error:err　　//获取文件的属性　　

-(BOOL)setAttributesOfItemAtPath:attr error:err　　//更改文件的属性

2.使用目录

-(NSString *)currentDirectoryPath　　//获取当前目录

-(BOOL)changeCurrentDirectoryPath:path　　//更改当前目录

-(BOOL)copyItemAtPath:from toPath:to error:err　　//复制目录结构（to不能是已存在的）

-(BOOL)createDirectoryAtPath:path withIntermediateDirectories:(BOOL)flag attribute:attr　　//创建一个新目录

-(BOOL)fileExistAtPath:path isDirectory:(BOOL*)flag　　//测试文件是不是目录（flag中储存结果YES/NO）

-(NSArray *)contentsOfDirectoryAtPath:path error:err　　//列出目录内容

-(NSDirectoryEnumerator *)enumeratorAtPath:path　　//枚举目录的内容

-(BOOL)removeItemAtPath:path error:err　　//删除空目录

-(BOOL)moveItemAtPath:from toPath:to error:err 　　//重命名或移动一个目录（to不能是已存在的）

3、常用路径工具方法

+(NSString *)pathWithComponens:components　　//根据components中的元素构造有效路径

-(NSArray *)pathComponents　　//析构路径，获得组成此路径的各个部分

-(NSString *)lastPathComponent　　//提取路径的最后一个组成部分

-(NSString *)pathExtension　　//从路径的最后一个组成部分中提取其扩展名

-(NSString *)stringByAppendingPathComponent:path　　//将path添加到现有路径的末尾

-(NSString *)stringByAppendingPathExtension:ext　　//将指定的扩展名添加到路径的最后一个组成部分

-(NSString *)stringByDeletingLastPathComponent　　//删除路径的最后一个组成部分

-(NSString *)stringByDeletingPathExtension　　//从文件的最后一部分删除扩展名

-(NSString *)stringByExpandingTileInPath　　　//将路径中代字符扩展成用户主目录（~）或指定用户的主目录（~user）

-(NSString *)stringByresolvingSymlinksInPath　　//尝试解析路径中的符号链接

-(NSString *)stringByStandardizingPath　　//通过尝试解析~、..（父目录符号）、.（当前目录符号）和符号链接来标准化路径

4、常用的路径工具函数

NSString* NSUserName(void)　　//返回当前用户的登录名

NSString* NSFullUserName(void)　　//返回当前用户的完整用户名

NSString* NSHomeDirectory(void)　　//返回当前用户主目录的路径

NSString* NSHomeDirectoryForUser(NSString* user)　　//返回用户user的主目录

NSString* NSTemporaryDirectory(void)　　//返回可用于创建临时文件的路径目录

5、常用的IOS目录

Documents（NSDocumentDirectory）　　//用于写入应用相关数据文件的目录，在ios中写入这里的文件能够与iTunes共享并访问，存储在这里的文件会自动备份到云端

Library/Caches（NSCachesDirectory）　　//用于写入应用支持文件的目录，保存应用程序再次启动需要的信息。iTunes不会对这个目录的内容进行备份

tmp（use NSTemporaryDirectory（））　　//这个目录用于存放临时文件，只程序终止时需要移除这些文件，当应用程序不再需要这些临时文件时，应该将其从这个目录中删除

Library/Preferences　　//这个目录包含应用程序的偏好设置文件，使用 NSUserDefault类进行偏好设置文件的创建、读取和修改