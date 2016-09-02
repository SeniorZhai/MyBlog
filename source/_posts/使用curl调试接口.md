title: 使用curl调试接口
date: 2014-11-18 17:34:18
categories: Prose
tags: [Api,调试,curl]
---
curl是一个向服务器传输数据的工具，支持http、ftp、ftps、scp、sftp、tftp、telnet协议
<!--more-->
## 打开网页
```shell
curl http://www.baidu.com/
```
也可以保存
```shell
curl http://www.baidu.com > ./buidu.html
```
或者
```shell
curl -o ./baidu.html http://www.baidu.com
```
## GET请求
```shell
curl http://www.baidu.com/s?wd=curl
```
## POST请求
```shell
curl -d "name=test&page=1" http://www.baidu.com
```
## 展示Header
只会显示头部内容
```shell
curl -I http://www.baidu.com
```
## 显示通信过程
```shell
curl -v www.baidu.com
```
如果需要详细的信息还可以使用
```shell
curl --trace output.txt www.baidu.com
# 或者
curl --trace-ascii output.txt www.baidu.com
```
## HTTP方法
curl默认是GET方法，使用`-X`参数可以支持其他动词
```shell
curl -X POST www.baidu.com
curl -X DELETE www.baidu.com
```
## Referer字段
在http request头部信息中提供referer字段，表示你是从哪里跳转过来的
```shell
curl --referer http://www.example.com http://www.example.com
```
## User Agent字段
User Agent用来表示客户端信息
iPhon4的User Agent是
	Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_0 like Mac OS X; en-us) AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8A293 Safari/6531.22.7

```shell
curl --user-agent "[User Agent]" [URL]
```
## 增加头部信息
```shell
curl --header "Content-Type:application/json" http://example.com
```