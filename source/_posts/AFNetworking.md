title: AFNetworking
date: 2014-03-26 13:00:34
categories: iOS
tags: [iOS]
---
![](https://github.com/zt19916161/blog/raw/matser/Image/14032601.png)
AFNetworking是一个非常流行的网络库，适用于iOS以及Mac OS X，具有良好的架构，丰富的api，以及模块化化构建方式，使得使用起来非常轻松。

##使用CocoaPods安装
###Podfile
```
platform :ios, '7.0'
pod "AFNetworking", "~> 2.0"
```

##环境要求
Xcode 5、iOS 6.0及以上、Mac OS 10.8以上

##用法
###AFHTTPRequestOperationManager
`AFHTTPRequestOperationManager`封装了Web应用程式HTTP通信的功能，包括编辑请求、响应序列化、网络可达性监控和安全系，以及请求管理的常见模式。

GET请求
```Objective-C
AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
[manager GET:@"http://example.com/resources.json" parameters:nil success:^(AFHTTPRequestOperation *operation, id responseObject) {
    NSLog(@"JSON: %@", responseObject);
} failure:^(AFHTTPRequestOperation *operation, NSError *error) {
    NSLog(@"Error: %@", error);
}];
```
POST请求
```Objective-C
AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
NSDictionary *parameters = @{@"foo":@"bar"};
[manager GET:@"http://example.com/resources.json" parameters:nil success:^(AFHTTPRequestOperation *operation, id responseObject) {
    NSLog(@"JSON: %@", responseObject);
} failure:^(AFHTTPRequestOperation *operation, NSError *error) {
    NSLog(@"Error: %@", error);
}];
```
POST多部分请求
```Objective-C
AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
NSDictionary *parameters = @{@"foo":@"bar"};
NSURL *filePath = [NSURL fileURLWithPath:@"file://path/to/image.png"];
[manager POST:@"http://example.com/resources.json" parameters:parametes construtingBodyWithBlock:^(id<AFMultipartFormData> formData){
	[formData appendPartWithFileUrl:filePath name:@"image" error:nil];
} success:^(AFHTTPRequestOperation *operation,id responseObject){
	NSLog(@"Success:%@",responseObject);
}failure:^(AFHTTPRequestOperation *operation,NSError *error)){
	NSLog(@"Error:%@",error);
}];
```
###AFURLSessionManager
`AFURLSessionManager`根据指定的`NSURLSessionConfiguration`对象创建、管理`NSURLSession`，实现了`<NSURLSessionTaskDelegate>`，`<NSURLSessionDataDelegate>`，`<NSURLSessionDownloadDelegate>`和`<NSURLSessionDelegate>`委托。

创建下载任务
```Objective-C
NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];

NSURL *URL = [NSURL URLWithString:@"http://example.com/download.zip"];
NSURLRequest *request = [NSURLRequest requestWithURL:URL];

NSURLSessionDownloadTask *downloadTask = [manager downloadTaskWithRequest:request progress:nil destination:^NSURL *(NSURL *targetPath, NSURLResponse *response) {
    NSURL *documentsDirectoryPath = [NSURL fileURLWithPath:[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject]];
    return [documentsDirectoryPath URLByAppendingPathComponent:[response suggestedFilename]];
} completionHandler:^(NSURLResponse *response, NSURL *filePath, NSError *error) {
    NSLog(@"File downloaded to: %@", filePath);
}];
[downloadTask resume];
```

创建下载任务
```Objective-C
NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];

NSURL *URL = [NSURL URLWithString:@"http://example.com/upload"];
NSURLRequest *request = [NSURLRequest requestWithURL:URL];

NSURL *filePath = [NSURL fileURLWithPath:@"file://path/to/image.png"];
NSURLSessionUploadTask *uploadTask = [manager uploadTaskWithRequest:request fromFile:filePath progress:nil completionHandler:^(NSURLResponse *response, id responseObject, NSError *error) {
    if (error) {
        NSLog(@"Error: %@", error);
    } else {
        NSLog(@"Success: %@ %@", response, responseObject);
    }
}];
[uploadTask resume];
```

创建上传任务多任务请求
```Objective-C
NSMutableURLRequest *requst = [[AFHTTPRequestSerializer serializer] multipartFormRequestWithMethod:@"POST" URLString:@"http://example.com/upload" parameters:nil constructingBodyWithBlock:^(id<AFMultipartFormData> formData) {
	[formData appendPartWithFileURL:[NSURL fileURLWithPath:@"file://path/to/image.jpg"] name:@"file" fileName:@"filename.jpg" mimeType:@"image/jpeg" error:nil];
}error:nil];

AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfigguration:[NSURLSessionConfiguration defaultSessionConfiguration]];
NSProgress *progress = nil;
NSURLSessionUploadTask *uploadTask = [Manager uploadTaskWithStreamedRequest:request progress:&progress completionHandler:^(NSURLResponse *response,id responseObject,NSError *error){
	if(error){
		NSLog(@"Error:%@",error);
	}else{
		NSLog(@"%@,%@",response,responseObject);
	}
}];
[uploadTask resume];
```

创建数据任务
```Objective-C
NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];

NSURL *URL = [NSURL URLWithString:@"http://example.com/upload"];
NSURLRequest *request = [NSURLRequest requestWithURL:URL];

NSURLSessionDataTask *dataTask = [manager dataTaskWithRequest:request completionHandler:^(NSURLResponse *response, id responseObject, NSError *error) {
    if (error) {
        NSLog(@"Error: %@", error);
    } else {
        NSLog(@"%@ %@", response, responseObject);
    }
}];
[dataTask resume];
[ dataTask  简历];
```
###请求序列化
创建序列化URL字符串
```Objective-C
NSString * URLString = @"http://example.com";
NSDictionary * parmeters = @{@"foo":@"bar",@"baz":@[@1,@2,@3]};
```
查询参数列表
```objective-c
[[AFHTTPRequestSerializer serializer] requestWithMethod:@"POST" URLString:URLString parameters:parameters];
```
 	POST http://example.com/
    Content-Type: application/x-www-form-urlencoded

    foo=bar&baz[]=1&baz[]=2&baz[]=3

###JSON参数
```objective-c
[[AFJSONRequestSerializer serializer] requestWithMethod:@"POST" URLString:URLString parameters:parameters];
```

  	POST http://example.com/
    Content-Type: application/json

    {"foo": "bar", "baz": [1,2,3]}

---
###网络可达性管理
`AFNetworkReachabilityManager`监控网络的可达性。
```objective-c
[[AFNetworkReachabilityManager sharedManager] setReachabilityStatusChangeBlock:^(AFNetworkReachabilityStatus status) {
    NSLog(@"Reachability: %@", AFStringFromNetworkReachabilityStatus(status));
}];
```
可达URL的主机
```objective-c
NSURL *baseURL = [NSURL URLWithString:@"http://example.com/"];
AFHTTPRequestOperationManager *manager = [[AFHTTPRequestOperationManager alloc] initWithBaseURL:baseURL];

NSOperationQueue *operationQueue = manager.operationQueue;
[manager.reachabilityManager setReachabilityStatusChangeBlock:^(AFNetworkReachabilityStatus status) {
    switch (status) {
        case AFNetworkReachabilityStatusReachableViaWWAN:
        case AFNetworkReachabilityStatusReachableViaWiFi:
            [operationQueue setSuspended:NO];
            break;
        case AFNetworkReachabilityStatusNotReachable:
        default:
            [operationQueue setSuspended:YES];
            break;
    }
}];
```

---