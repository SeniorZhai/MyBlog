title: Foundation框架(一)
date: 2014-02-28 12:46:48
categories: iOS
tags: [iOS]
---
## 字符串（NSString与NSMutableString）
- NSString代表字符串序列不可变的字符串，NSMutableString代表序列可变的字符串
- NSString常用方法
```
NSString *str = @"This is a String!";//以字符串常量创建字符串
NSString *str = [[NSString alloc] init];
str = @"This is a String!";//创建空字符串，赋值
NSString *str = [[NSString alloc] initWithString:@"This is a String!"//初始化字符串
char *Cstring = "This is a String!"
NSString *str = [[NSString alloc] initWithCString:Cstring];//以标准c字符串初始化创建字符串
NSString *str = [[NSString alloc] stringWithFormat:@"%s is a String","This"];//格式化创建字符串
NSString *path = [[NSBundle mainBundle] pathForResouce:@"text" ofType:@"txt"];
NSString *str = [[NSString alloc] initWithContentsOfFile:path];//从文件创建字符串
[str writeToFile:path atomically:YES];//写入文件
BOOL result = [str1 isEqualToString:str2];//判断相等与否
str = [str stringByAppendingString:@"iOS!"];//追加字符串
char* cstr = [str UTF8String];//获取字符串对应的C风格字符串
[str length];//获取字符串长度
[str lengthOfBytesUsingEncoding:NSUTF8StringEncoding];//str按UTF-8字符集解码后字节数
[str substringToIndex:10];//获取str的前10个字符组成的字符串
[str substringFromIndex:5];/获取从第5个字符串开始，与后面字符组成的字符串
[str substringWithRange:NSMakeRange(5,15)];//获取str从第5个字符开始，到第15个字符组成的字符串
[str rangeOfString:@"ios"];//返回ios在字符串中出现的位置
[str uppercassString];//str所有字符转为大写
```
- NSMutableString可改变序列
- NSMutableString常用方法
```
[str appendString:@",iOS"];//追加固定字符串
[str appendFormat:@"%s is ios",@",iOS"];//追加字符串
[str insertString:@"insert" atIndex:6];//指定位置插入字符串
[str deleteCharactersInRange:NSMakeRange(6,12)];//删除6到12位置的所有字符串
[str replaceCharactersInRange:NSMakeRange(6,9) withString:@"Objective-C"];//替换6到9位置的字符串
```

## 日期与时间（NSDate）
- 常用方法
```
NSDate* date1 = [NSDate date];//获取代表当前日期、时间的NSDate
NSDate* date2 = [[NSDate alloc] initWithTimeIntervalSinceNow:3600*24];//获取从当前时间开始3天之后的日期
NSDate* date3 = [NSDate dateWithTimeIntervalSince1970:3600*24*366*20];//从1970年1月1日开始，20年之后的日期
NSLo
cale* cn = [NSLocale currentLocale];//获取本地位置
[dete1 descriptionWithLocal:cn];//过去NSDate在当前位置对应的字符串
[date1 earlieDate:date2];//获取两个日期之间较早的日期
[date laterDate:date2];//获取两个日期之间较晚的日期
switch([date1 compare date2])
{
    case NSOrderedAscending:
        break;  //date1在之前
    case NSOrderedSame:
        break;  //date1与date2日期相同
    case NSOrderedDescending:
        break;  //date1在之后
}
[date1 timeIntervalSinceDate:date2];//时间差
[date1 timeIntervalSinceNow];//与现在时间差
```
    + NSLocale待遇一个语言、国际环境

## 日期格式器（NSDateFormatter）
- NSDateFoematter代表一个日期格式器，用来完成NSDate与NSString之间的转换
1. 创建一个NSDateFormatter对象
2. 调用NSDateFormatter的`setStyle:`、`setTimeStyle:`方法设置格式化日期、时间的风格，日期、时间风格支持如下的枚举值：
    + NSDateFormatterNoStyle:不显示日期、时间的风格
    + NSDateFormatterShortStyle:显示“短”的日期、时间风格
    + NSDateFormatterMediumStyle:显示“中等”的日期、时间风格
    + NSDateFormatterLongStyle:显示“长”的日期、时间风格
    + NSDateFormatterFullStyle:显示“完整”的日期、时间风格
3. NSDate转换成NSString可以调用NSDateFormatter的stringFromDate:方法执行格式化即可
4. NSString转换成NSDate可以调用NSDateFormatter的dateFromString:方法执行格式化即可
```
// 自定义格式式模板
NSDateFormatter* df = [[NSDateFormatter alloc]init];
[df setDateFormatter:@"公元yyyy年MM月DD日HH时mm分"];
NSLog(@"%@",[df stringFromDate:date]);
```

## 日历（NSCalendar）与日期组件（NSDateComponents）
- NSCalender对象用于处理NSDate对象所包含的各个字段的数据
- NSCalendar常用方法
    + (NSDateComponents*)components:fromDate:从NSDate提取年、月、日、时、分、秒各时间字段的信息。
    + dateFromComponents:(NSDateComponents*)comps:使用comps对象包含的年、月、日、时、分、秒各时间字段的信息来创建NSDate.

## 定时器（NSTimer）
1. 调用NSTimer的scheduledTimerWithTimeInterval:invocation:repeats:或scheduledTimerWithTimeInterval:target:selector:userInfo:repeats:类方法来创建NSTimer对象。
    + timerInterval:指定每个多少秒执行一次任务。
    + invocation或target与selector:指定重复执行的任务。如果指定target和selector参数，则指定用某个对象的特定方法作为重复执行的任务；如果指定invacation参数，该参数需要传入一个NSInvocation对象，其中NSInvocation对象也是封装target和selector的，其中也是指定用某个对象的特定方法作为重复执行的任务。
    + userInfo:该参数用于传入额外的附加信息。
    + repeats:该参数需要指定BOOL值，该参数控制是否需要重复执行任务。
2. 为第一步的任务编写方法。
3. 销毁定时器，调用定时器的invalidate方法即可。
 
## 对象复制
- NSObject类提供了copy和mutableCopy方法，通过这两个方法即可复制已有对象的副本。
- copy方法总是返回不可修改的副本，及时改对象本身是可修改的。
- mutableCopy方法用于复制对象的可变副本，即便对象本事是不可修改的，如NSString，返回的是NSMutableString对象。

### NSCopying与NSMutableCopy协议
- 为了保证自定义类能够使用copy、mutableCopy方法，需要做如下的事
    + 让该类实现NSCopying（NSMutableCopying）协议
    + 让该类实现copyWithZone:(mutableCopyWithZone:)方法
```Objective-C
-(id)copyWithZone:(NSZone*)zone
{
    ZTApple apple= [[[self class] allocWithZone:zone]init];
    ZTApple.color = self.color;
    ZTApple.weight = self.weight;
    return apple;
}
// 如果父类已经实现了NSCopying协议，那么代码就可简化
-(id)copyWithZone:(NSZone*)zone
{
    id obj = [super copy];
    //...
    return obj;
}
```

### 浅复制(shallow copy)与深复制(deep copy)
- 当对象的实例变量是指针变量时，程序只是复制该指针的地址，而不是真正复制指针指向的对象，这种方式被称为“浅复制”，对浅复制而言，在内存中复制了两个对象，这两个对象的指针变量将会指向同一个对象，也就是两个对象存在公用的部分。
- 深复制不仅会复制对象本身，而且会“递归”复制每个指针类型的实例变量，直到两个对象没有任何公用的部分。
```Objective-C
-(id)copyWithZone:(NSZone*)zone
{
    ZTApple apple= [[[self class] allocWithZone:zone]init];
    ZTApple.color = [self.color mutableCopy];
    ZTApple.weight = self.weight;
    return apple;
}
```
- 一般来说，Foundation框架中的类大部分只实现了浅复制。
#### setter方法的copy选项
- setter的copy只实现copy方法，得到的回事一个不可变的副本。