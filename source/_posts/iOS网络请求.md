title: iOS网络请求
date: 2014-09-22 15:14:26
categories: iOS
tags: [网络请求,POST,GET]
---
iOS中的基本网络请求如下
<!--more-->
## 同步
同步访问网络不考虑延迟问题
```objective-c
// 同步Get
NSString *urlStr = [NSString stringWithFormat:@"%@",@"https://wap.baidu.com"];

NSString *newUrlStr = [urlStr stringByAddingPercentEscapesUsing:NSUTF8StringEncoding];

NSURL *url = [NSURL URLWithString:newUrlStr];

// 创建网络请求
NSURLRequest *request = [NSURLRequest requestWithURL:url cachePolicy:NSURLRequestReloadIgnoringLocalCacheData timeoutInterval:10];
// 创建同步连接
NSURLResponse *response = nil;

NSError *error = nil;

NSData *data = [NSURLConnection sendSynchronousRequest:request returningResponse:&response error:&error];
// 同步Post
NSString *urlStr = [NSString stringWithFormat:@"%@",kVideoURL];

NSURL *url = [NSURL URLWithString:urlStr];

NSString *parmStr = @"method=album.channel.get&appKey=myKey&format=json&channel=t&pageNo=1&pageSize=10";

NSData *pramData = [parmStr dataUsingEncoding:NSUTF8StringEncoding];

// 设置请求体
[request setHTTPBody:pramData];

// 设置请求方式
[request setHTTPMethod:@"POST"];

NSData *data = [NSURLConnection sendSynchronousRequest:request returningResponse:nil error:nil];
```
## 异步
异步访问网络
```objective-c
NSString *urlStr = [NSString stringWithFormat:@"http://image.zcool.com.cn/56/13/1308200901454.jpg"];

NSString *newStr = [urlStr stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];

NSURL *url = [NSURL URLWithString:newStr];

NSURLRequest *requst = [NSURLRequest requestWithURL:url cachePolicy:NSURLRequestReloadIgnoringLocalCacheData timeoutInterval:10];

//异步链接(形式1,较少用)

[NSURLConnection sendAsynchronousRequest:requst queue:[NSOperationQueue mainQueue] completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {

self.imageView.image = [UIImage imageWithData:data];
// 解析
NSDictionary *dic = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];

NSLog(@"%@", dic);

}];

// POST请求

NSString *urlString = [NSString stringWithFormat:@"%@",kVideoURL];

//创建url对象

NSURL *url = [NSURL URLWithString:urlString];

//创建请求

NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url cachePolicy:NSURLRequestReloadIgnoringLocalCacheData timeoutInterval:10];

//创建参数字符串对象

NSString *parmStr = [NSString stringWithFormat:@"method=album.channel.get&appKey=myKey&format=json&channel=t&pageNo=1&pageSize=10"];

//将字符串转换为NSData对象

NSData *data = [parmStr dataUsingEncoding:NSUTF8StringEncoding];

[request setHTTPBody:data];

[request setHTTPMethod:@"POST"];

//创建异步连接（形式二）

[NSURLConnection connectionWithRequest:request delegate:self];

// 一般的，当创建异步连接时， 很少用到第一种方式，经常使用的是代理方法。关于NSURLConnectionDataDelegate，我们经常使用的协议方法为一下几个：

// 服务器接收到请求时

- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response

{

}

// 当收到服务器返回的数据时触发, 返回的可能是资源片段

- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data

{

}

// 当服务器返回所有数据时触发, 数据返回完毕

- (void)connectionDidFinishLoading:(NSURLConnection *)connection

{

}

// 请求数据失败时触发

- (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error

{

	NSLog(@"%s", __FUNCTION__);

}
```
##  GET请求和POST请求的区别
1. GET请求的接口会包含参数部分，参数会作为网址的一部分，服务器地址与参数之间通过 ? 来间隔。POST请求会将服务器地址与参数分开，请求接口中只有服务器地址，而参数会作为请求的一部分，提交后台服务器。
2. GET请求参数会出现在接口中，不安全。而POST请求相对安全。
3.虽然GET请求和POST请求都可以用来请求和提交数据，但是一般的GET多用于从后台请求数据，POST多用于向后台提交数据。

## 同步和异步的区别
- 同步链接：主线程去请求数据，当数据请求完毕之前，其他线程一律不响应，会造成程序就假死现象。
- 异步链接：会单独开一个线程去处理网络请求，主线程依然处于可交互状态,程序运行流畅。