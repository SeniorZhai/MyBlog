title: 文件I/O
date: 2014-02-27 12:44:30
categories: iOS
tags: [iOS]
---
##NSData与NSMutableData
- 代表数据缓存区，作用有两个①对数据读取NSData、②输出NSData的数据
	+ 初始化方法(类方法以data开头，实例方法以init开头)
	    - `data:`：创建一个不包含任何数据的、空的NSData对象
	    - `dataWithBytes:length:`/`initWithBytesNoCopy:length:`：复制C数组所包含的数据类初始化NSData的数据
	    - `dataWithBytesNoCopy:length:`/`initWithBytesNoCopy:length:`：直接利用C数组所包含数据来初始化NSData对象。当该对象呗执行malloc方法销毁自己时，程序会释放该数组
	    - `dataWithBytesNoDopy:length:freeWhenDone:`/`initWithBytesNoCopy:length:freeWhenDone:`：直接利用C数组所包含的数据来初始化NSData对象。只有当最后一个参数为YES，且该对象被执行malloc方法销毁自己时，程序才会释放该C数组
	    - `dataWithContentsOfFile:`/`initWithContentsOfFile:`：直接读取文件内容，并利用文件内容来初始化NSData
	    - `dataWithContentOfURL:`/`initWithContentOfURL:`：直接读取URL关联的内容，并利用该URL关联的内容初始化NSData
	    - `dataWithData:`/`initWithData:`：直接使用另一个NSData所包含的数据来初始化先创建的NSData
    + 常用方法
        - `bytes:`：获取该NSData所包含的数据
        - `getBytes:length:`：获取NSData所包含的指定长度的数据
	    - `getBytes:range:`：获取NSData所包含的指定范围的数据
	    - `subdataWithRange:`：获取NSData所包含的指定范围的数据组成的NSData对象
	    - `writeToFile:atomically:`：将NSData的数据写入文件
	    - `writeToURL:atomically:`：将NSData的数据写入URL对应的资源

##NSFileManager
- NSFileManager代表文件管理器，可以执行文件的移动、复制、链接、删除文件或目录，同时提供一个配套的事件委托（NSFileManagerDelegate），确保文件操作后调用相对应的处理方法。文件名作为文件的唯一标识，可使用绝对路径或相对路径。
- NSFileManager访问属性和内容提供如下方法：

|**方法名**|**说明**|
|---|:---|
|fileExistsAtPath:|判断指定文件名对应的文件是否存在|
|fileExistsAtPath:isDirectoy:|判断指定文件名对应的文件或者目录是否存在，最后一个参数可用于返回该文件名是否为目录|
|isReadableFileAtPath:|判断指定目录下的文件是否可读|
|isWritableFileAtPath:|判断指定目录下的文件是否可写|
|isExcutableFileAtPath:|判断指定目录下的文件是否可执行|
|isDeletableFileAtPath:|判断指定目录下的文件是否可删除|
|componentsToDisplayForPath:|获取指定文件名对应文件的各个路径组件|
|displayNameAtPath:|获取指定文件名对应文件的简单文件名|
|attributesOfItemAtPath:error:|获取指定文件名对应文件的属性|
|attributesOfFileSystemForPath:error:|获取指定文件名对应的文件所在文件系统的属性|
|setAttributes:ofItemAtPath:error:|设置指定文件名对应文件的属性|
|contentsAtPath:|获取指定文件名对应文件的内容|
|contentsEqualAtPath:andPath:|判断两个文件名指定的内容是否相同|
- NSFileManager为创建、删除、移动、复制文件或目录提供了如下方法

|**方法名**|**说明**|
|---|:---|
|createDirectoryAtURL:withIntermediateDirectories:attributes:error:|根据指定的URL创建目录|
|createDirectoryAtPath:withIntermediateDirectories:attributes:error:|根据指定的路径创建目录|
|createFileAtPath:contents:attributes:|根据指定的文件路径、内容创建文件|
|removeItemAtURL:error:|删除指定URL对应的文件|
|removeItemAtPath:error:|删除指定路径对应的文件|
|copyItemAtURL:toURL:error:|根据指定的URL复制文件或目录|
|copyItemAtPath:toPath:error:|根据指定的路径复制文件或目录|
|moveItemAtURL:toURL:error:|根据指定URL移动文件或目录|
|moveItemAtPath:toPath:error:|根据指定路径移动文件或目录|
- NSFileManager查看目录包含内容提供了如下方法

|**方法名**|**说明**|
|---|:---|
|contentsOfDirectoryAtPath:error:||
|enumeratorAtPath:||
|subpathsOf

##NSURL
- URL(Uniform Resource Locator)对象表示同意资源定位器，它是指向互联网“资源”的指针。通常情况下，URL由协议名、主机、端口和资源路径组成，格式如下：
>scheme://host:port/path
