title: JSONModel的使用
date: 2014-12-13 13:14:54
categories: iOS
tags: [Json,Model,JSON解析]
---
<!--more-->
##  使用`JSONModel`
1. 创建一个新的Ocjective-C的类并继承`JSONModel`
2. 在头文件中声明Json中Keys同名属性
```objective-c
@import "JSONModel.h"

@interface CountryModel : JSONModel

@property (assign,nonatomic) int id;
@property (strong,nonatomic) NSString* country;
@property (strong,nonatomic) NSString* dialCode;
@property (assign,nonatomic) BOOL isInEurope;

@end
```
不需要在.m文件中填写任何东西

解析JSON字符串时
```objective-c
NString* json = ...
NSError* error = nil;
CountryModel* contryModel = [[CountryModel alloc] initWithString:json error:&error];
```

## 示例
### 基础的例子
```json
{
	"id":"123",
	"name":"Product name",
	"price":12.95
}
```
```ocjective-c
@interface ProductModel : JSONModel

@property (assign,nonatomic) int id;
@property (strong,nonatomic) NSString* name;
@property (assign,nonatomic) float price;

@end
```
### 嵌套类
```json
{
	"order_id":104,
	"total_price":13.45,
	"product":{
		"id":"123",
		"name":"Product name",
		"price":12.34
	}
}
```
```objective-c
@interface OrderModel : JSONModel

@property (assign,nonatomic) int order_id;
@property (assign,nonatomic) float total_price;
@property (strong,nonatomic) ProductModel* product;

@end
```

### 嵌套集合
```json
{
	"oder_id":104,
	"total_price":103.45,
	"products" : [
	{
		"id":"123",
		"name":"Product # 1",
		"price":12.95
	},
	{
		"id":"137",
		"name":"Product # 2",
		"price":82.95
	}
	]
}
```

```objective-c
@potocol ProductModel
@end

@interface ProductModel : JSONModel

@property (assign,nonatomic) int id;
@property (strong,nonatomic) NSString* name;
@property (assign,nonatomic) float price;

@end

@implementation ProductModel
@end

@interface OrderModel : JSONModel

@property (assign,nonatomic) int order_id;
@property (assign,nonatomic) float tital_price;
@property (strong,nonatomic) NSArray<ProductModel> *products;

@end

@implementation OrderModel
@end
```

### 修改Key值
```json
{
	"order_id":104,
	"order_details" : [
	{
		"name":"Product# 1",
		"price":{
			"usd":12.95
		}
	}
	]
}
```

```objective-c
@interface OrderModel : JSONModel
@property (assign,nonatomic) int id;
@property (assign,nonatomic) float price;
@property (strong,nonatomic) NSString* productName;
@end

@implementation OrderModel

+ (JSONKeyMapper *)keyMapper
{
	return [[JSONKeyMapper alloc] initWithDictionary:@{
		@"order_id":@"id",
		@"order_details.mame":@"productName",
		@"order_details.price":@"price"
	}];
}
@end
```
### 全局改变Key 
```objective-c
[JSONModel setGlobalKeyMapper:[
	[JSONKeyMapper alloc] initWithDictionary:@{
		@"item_id":@"ID",
		@"item_name":@"itemName"
	}]
];
```
### 使用JSON请求
```objective-c
[[JSONHTTPClient requestHeaders] setValue:@"MySecret" forKey:@"AuthorizationToken"];
[JSONHTTPClient postJSONFromURLWithString:@"http://mydomain.com/api"
								   params:@{@"postParma1":@"valuel"}
							   completion:^(id json,JSONModelError *err){

								}];
```

### JSON文本和Dictionary的转换
```objective-c
ProductModel* pm = [[ProductModel alloc] initWithString:jsonString error:nil];
pm.name = @"Changed Name";

NSDictionary* dict = [pm toDictionary];
NSString* string = [pm toJSONString];
```

## 自定义Date格式
```objective-c
@implementation JSONValueTransformer (CustomTransformer)

- (NSDate *)NSDateFromNSString:(NSString *)string {
	NSDateFormatter *formatter = [[NSDatrFormatter alloc] init];
	[formatter setDateFormat:APIDateFormat];
	return [formatter dateFromString:string];
}

- (NSString *)JSONObjectFromNSDate:(NSDate *)date {
	NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
	[formatter setDateFormat:APIDateFormat];
	return [formatter stringFromDate:date];
}

@end
```

	注：关于名字为id的属性，可以使用id命名，而且需要使用NSNumber或者NSString的类型来定义