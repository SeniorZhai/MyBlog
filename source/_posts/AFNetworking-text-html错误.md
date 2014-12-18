title: AFNetworking text/html错误
date: 2014-12-12 18:11:42
categories: iOS
tags: [AFNetworking]
---
因为AFNetworking从2.0默认不支持`text/html`
<!--more-->
在`AFURLResponseSerialization.m`的223行` self.acceptableContentTypes = [NSSet setWithObjects:@"application/json", @"text/json", @"text/javascript", nil];`中添加`text/html`