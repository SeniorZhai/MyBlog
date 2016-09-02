title: KVO
date: 2014-03-20 12:57:05
categories: iOS
tags: [iOS]
---
KVO(Key-Value Observing)，Objective-C提供的一种机制，当制定的对象被修改时，对象就会接受到通知。
## 使用捕捉
1. 注册，制定被观察者的属性
2. 实现回调方法
3. 移除观察

KVO涉及到的主要方法：
```
//	三个参数分别是监听对象、监听属性、监听选项
- (void)addObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath options:(NSKeyValueObservingOptions)options context:(void *)context;
//	删除监听器，监听的对象、监听的属性
- (void)removeObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath;
```

```Objective-C
@implementation ViewController
{
    UILabel *_name;
    UILabel *_age;
    UIButton *_b1;
    UIButton *_b2;
}

- (void)viewDidLoad
{
    [super viewDidLoad];
	_myData= [[MyData alloc] init];
    [_myData setValue:@"name" forKey:@"name"];
    [_myData setValue:@"22" forKey:@"age"];
    [_myData addObserver:self forKeyPath:@"age" options:NSKeyValueObservingOptionNew|NSKeyValueObservingOptionOld context:NULL];
    [_myData addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionNew|NSKeyValueObservingOptionOld context:NULL];

    CGSize size = self.view.bounds.size;

    _name = [[UILabel alloc]initWithFrame:CGRectMake(size.width/2 - 75, 100, 150, 30 )];
    _name.textColor = [UIColor redColor];
    _name.text = [_myData valueForKey:@"name"];
    _name.textAlignment = NSTextAlignmentCenter;
    [self.view addSubview:_name];

    _age = [[UILabel alloc]initWithFrame:CGRectMake(size.width/2 - 50, 140, 100, 30 )];
    _age.textColor = [UIColor redColor];
    _age.text = [[NSString alloc]initWithFormat:@"%@",[_myData valueForKey:@"age"]];
    _age.textAlignment = NSTextAlignmentCenter;
    [self.view addSubview:_age];
    
    _b1 = [UIButton buttonWithType:UIButtonTypeRoundedRect];
    [_b1 setTitle:@"Push" forState:UIControlStateNormal];
    _b1.frame = CGRectMake(size.width/2 - 50, size.height/2 + 100, 100, 30);
    [_b1 addTarget:self action:@selector(buttonAction1) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:_b1];
    
    _b2 = [UIButton buttonWithType:UIButtonTypeRoundedRect];
    [_b2 setTitle:@"Push" forState:UIControlStateNormal];
    _b2.frame = CGRectMake(size.width/2 - 50, size.height/2 + 140, 100, 30);
    [_b2 addTarget:self action:@selector(buttonAction2) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:_b2];
}
-(void) buttonAction1
{
    unsigned int random = arc4random_uniform(999);
    _myData.name = [[NSString alloc]initWithFormat:@"随机数字:%d",random];
}
-(void) buttonAction2
{
    unsigned int age = arc4random_uniform(100);
    _myData.age = age;
}
-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context
{
    if([keyPath isEqualToString:@"age"])
    {
        _age.text = [[NSString alloc]initWithFormat:@"%@",[_myData valueForKey:@"age"]];
    } else if([keyPath isEqualToString:@"name"])
    {
        _name.text = _myData.name;
        
    }
}
```