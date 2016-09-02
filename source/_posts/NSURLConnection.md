title: NSURLConnection
date: 2014-04-09 13:05:53
categories: iOS
tags: [iOS]
---
1. 先创建一个NSURL
2. 在通过NSURL创建NSURLRequest，可以指定缓存规则和超时时间
3. 创建NSURLConnection实例，指定NSURLRequest和delegate对象，如果创建失败，则返回nil，如果创建成功则创建一个NSMutableData的实例用来存储数据。

```
NSURLRequest *request = [NSUELRequest requestWithURL:[NSURL URLWithString:@"http://www.sina.com.cn/"] cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0];
NSURLConnection *connection = [[NSURLConnection alloc] initWithRequest:request delegate:self];
if(connection){
	// 创建成功
}
else{
	// 创建失败
}

# pragma mark- NSUrlConnectionDelegate methods
- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response
{
    //接受一个服务端回话，再次一般初始化接受数据的对象
}

- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data
{
	// 每个中间data
}

- (void)connectionDidFinishLoading:(NSURLConnection *)connection
{
    // 连接结束

}

- (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error
{
    // 链接错误
}
```