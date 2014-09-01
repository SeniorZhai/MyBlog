title: NSURLSession
date: 2014-03-09 12:51:59
categories: iOS
tags: [iOS]
---
- NSURLSession是iOS7中新的网络接口，它与NSURLConnection是并列的，程序在前台时，NSURLSession与NSURLConnection可以互为替代工作，注意，如果用户强制将小横须关闭，NSURLSession会断掉。

- NSURLSession的功能：
1. 通过URL将数据下载到内存
2. 通过URL将数据下载到文件系统
3. 将数据上传到指定URL
4. 在后台完成上述功能

- 工作流程
1.创建一个NSURLSessionConfiguration，用于第二部创建NSSession时设置工作模式和网络设置，工作模式分为：
	+ 一般模式（default）：工作模式类似于NSURLConnection，可以使用缓存的Cache，Cookie，鉴权
	+ 及时模式（ephemeral）：不使用缓存的Cache，Cookie，鉴权
	+ 后台模式（background）：在后台完成下载，创建Configuration对象的时候需要给一个NSString的ID用于跟踪完成工作的Session是哪一个
网络设置：参靠NSURLConnection中的设置项。

1. 创建一个NSURLSession。系统提供了两个创建方法
	+ sessionWithConfiguration:
	+ sessionWithConfiguration:delegate:delegateQueue:

	+ 第一个粒度较低就是根据刚才创建的Configuration创建一个Session，系统默认创建一个新的OperationQueue处理Session的消息。
 
	+ 第二个粒度比较高，可以设定回调的delegate（注意这个回调delegate会被强引用），并且可以设定delegate在哪个OperationQueue回调，如果我们将其设置为[NSOperationQueue mainQueue]就能在主线程进行回调非常的方便。
2. 创建一个NSURLRequest调用刚才的NSURLSession对象提供的Task函数，创建NSURLSessionTask
	+ 根据智能不同Task有三个子类：
		- NSURLSessionUploadTask:上传用的Task，传完以后不会再下载返回结果；
		- NSURLSessionDownloadTask:下载用的Task；
		- NSURLSessionDataTask:可以上传内容，上传完成后再进行下载;
	+ 得到的Task，调用resume开始工作
3. 如果是细粒度的Session调用，Session与Delegate会在指定的OperationQueue中进行交互，以下载例子，交互的顺序图如下（假设不需要鉴权，即非HTTPS请求）：
![](https://github.com/zt1991616/blog/raw/master/Image/14030901.jpg)
4. 当不再需要连接调用Session的invaildateAndCancel直接关闭，或者调用finishTasksAndInvalidate等待当前Task结束后关闭。这时Delegate会收到URLSession:didBeComeInvaliWithErro:这个事件。Delegge收到这个事件之后会被解引用。
5. 如果是一个BackgroundSession，在Task执行的时候，用户切到后台，Session会和ApplicationDelegate做交互，当程序切到后台后，在BackgroundSession中的Task还会继续下载
    
1）当加入了多个Task，程序没有切换到后台
这种情况Task会按照NSURLSessionConfiguration设置正常下载，不和和ApplicationDelegate有交互。

2）当加入了多个Task，程序切换到后台，所有Task都完成下载。
在切换到后台之后，Session的Delegate的application:handleEventForBackgroundURLSession:completionHandler:回调，之后“汇报”下载工作，对于每一个后台下载的Task调用Session的Delegate中的URLSession:downloadTask:didFinishDownloadingToURL:（成功的话）和URLSession:task:didCompleteWithError:(成功或者失败都会调用)
之后调用Session的Delegate回调URLSessionDidFinishEventsForBackgroundURLSession
- **注意：在ApplicationDelegate被唤醒后，会有个参数ComplietionHandler，这个参数是个Block，这个参数要在后面Session的Delegate中didFinish的时候调用一下，如下：**

```
@implementation APLAppDelegate 
 
  
 
- (void)application:(UIApplication *)application handleEventsForBackgroundURLSession:(NSString *)identifier 
 
  completionHandler:(void (^)())completionHandler 
 
{ 
 
    BLog(); 
 
    /* 
 
     Store the completion handler. The completion handler is invoked by the view controller's checkForAllDownloadsHavingCompleted method (if all the download tasks have been completed). 
 
     */ 
 
      self.backgroundSessionCompletionHandler = completionHandler; 
 
} 
 
//…… 
 
@end 
 
  
 
//Session的Delegate 
 
@implementation APLViewController 
 
  
 
- (void)URLSessionDidFinishEventsForBackgroundURLSession:(NSURLSession *)session 
 
{ 
 
    APLAppDelegate *appDelegate = (APLAppDelegate *)[[UIApplication sharedApplication] delegate]; 
 
    if (appDelegate.backgroundSessionCompletionHandler) { 
 
        void (^completionHandler)() = appDelegate.backgroundSessionCompletionHandler; 
 
        appDelegate.backgroundSessionCompletionHandler = nil; 
 
        completionHandler(); 
 
    } 
 
  
 
    NSLog(@"All tasks are finished"); 
 
} 
 
@end 
```
3）当加入了多个Task，程序切换到后台，下载完成了几个Task，然后用户又切换到前台。（程序没有退出）
- 切换到后台之后，Session的Delegate仍然收不到消息，在下载完成几个Task之后再切换到前台，系统会先汇报已经下载完成的Task的情况，然后继续下载没有下载完成的Task。

4）当加入了多个Task，程序切到后台，几个Task已经完成，但还有Task还没有下载完的时候关掉退出程序然后再进入程序的时候。（程序退出了）

