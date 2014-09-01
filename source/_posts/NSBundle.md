title: NSBundle
date: 2014-02-27 12:45:49
categories: iOS
tags: [iOS]
---
- NSBundle用来访问应用自包含的资源文件。
- 获取NSBundle对象，一般会调用该类的mainBundle方法。
    + URLForResource:withExtension:subdirectory:根据`资源名`、`扩展名`从指定`子目录`获取对用资源的URL
    + URLForResource:withExtension:根据`资源名`、`扩展名`获取该资源对应的URL
    + pathForResouce:ofType:根据`资源名`、`类型名`获取该资源对应的路径
    + URLsForResoucesWithExtension:subdirectory:获取指定`子目录`下匹配特定`扩展名`的所有资源对应的URL组成的数组
    + pathForResouces:ofType:inDirectory:从指定的子目录下，根据`资源名`,`类型名`获取该资源对应的路径
    + pathForResoucesOfType:inDirectory:获取指定子目录下，匹配特定类型名的所有资源对应的路径组成的数组
    + resoucePath:直接根据完整的资源路径来获取对应的资源
```Objective-C
NSString* filePath = [[NSBundle mainBundle] pathForResource:@"test" ofType:@"txt"];
NSString* content = [NSString stringWithContentsOfFile:filePath encoding:NSUTF8StringEncoding error:nil];
```