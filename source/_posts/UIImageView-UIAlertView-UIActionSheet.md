title: UIImageView-UIAlertView-UIActionSheet.
date: 2014-02-21 14:35:32
categories: iOS
tags: [UIImageView,UIAlertView,UIActionSheet.]
---
##UIImageView
- 一个显示图片控件，直接继承了UIView基类，没有继承UIControl，不能接受用户输入，不能与用户交互。
- 常用属性
`image`:访问或设置该控件显示的图片
`highlighteImage`:访问或设置该控件出于高亮状态时显示的图片
- UIImage还可以使用动画显示一组图片
`animationImages`:访问或者设置该UIImageView需要动画显示的多张图片，该属性的值为NSArray对象。
`highlightedAnimationImages`:访问或设置该UIImageView高亮状态下需要动画显示的多张图片，该属性的值为NSArray对象。
`animationDuration`:访问或设置该UIImageView的动画持续时间。
`animationRepeatCout`:访问或设置该UIImageView的动画重复次数。
`startAnimating`:开始播放动画。
`stopAnimating`:停止播放动画。
`isAnimationg`:该方法判断该UIImageView是否正在播放动画。
- 缩放模式
`Scale To Fill`:不保持纵横比缩放图片，使图片完全适应该UIImageView控件。
`Aspect Fit`:保持纵横比缩放图片，使图片的长边能完全显示出来。也就是说，可以完整地将图片显示出来。
`Aspect Fill`:保持纵横比缩放图片，只保证短边完全显示出来。
`Center`:不缩放图片，只显示图片的中间区域。
`Top`:不缩放图片，只显示图片的顶部区域。
`Bottom`:不缩放图片，只显示图片的底部区域。
`Left`:不缩放图片，只显示图片的左边区域。
`Right`:不缩放图片，只显示图片的右边区域。
`Top Right`:不缩放图片，只显示图片的右上边区域。
`Top Left`:不缩放图片，只显示图片的左上边区域。
`Bottom Left`:不缩放图片，只显示图片的左下边区域。
`Bottom Right`:不缩放图片，只显示图片的右下边区域。
##UIAlertView(警告框)
- UIAlertView
    + 显示在屏幕中央的弹出式警告框。
    + 基本用法
        1. 创建`UIAlertView`,指定标题、消息内容、包含多少个按钮、指定UIAlertViewDelegate委托对象。
        2. 调用`UIAlertView`显示出来。
        3. 实现`UIAlertViewDelegate`协议中的方法。
    + 实例代码
    ```Objective-C
    -(void)viewDidLoad
    {
        [super viewDidLoad];
    }
    -(IBAction)clicked:(id)sender
    {
        UIAlertView *alert = [[UIAlertView alloc]
            initWithTitle:@"提示"
            message:@"警告框信息"
            deledgate:self
            cancelButtonTitle:@"确定"
            otherButtonTitles:@"按钮一",@"按钮二",@"按钮三",nil];
        [alert show];
    }
    -(void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex
    {
        NSString* msg = [NSString stringWithFormat:@"你点击了第%d按钮",buttonIndex];
        UIAlertView *alert = [[UIAlertView alloc]
            initWithTitle:@"提示"
            message:msg
            delegate:nil
            cancelButtonTitle:@"确定"
            otherButtonTitles:nil];
        [alert show]
    }
    ```
    + UIAlertViewDelegate协议中常用的方法
```Objective-C
-(void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex:
//当用户点击该警告框中某个按钮时激发该方法，buttonIndex参数代表用户单击按钮的索引，索引从0开始。
-(void)willPresenAlertView:(UIAlertView *)alertView:
//当该警告框将要显示出来时将会激发该方法。
-(void)didPresenAlertView:(UIAlertView *)alertView:
//当该警告框将要显示出来后将会激发该方法。
-(BOOL)alertViewShouldEnableFirstOtherButton:(UIAlertView *)alertView:
//当该警告框第一个非Cancel按钮被启用时激发该方法。
-(void)alertView:(UIAlertView *)alerView willDismissWithButtonIndex:(NSInteger)buttonIndex:
//当用户单击某个按钮将要隐藏该警告框时激发该用法。
-(void)alertViewCancel:(UIAlertView *)alertView didDismissWithButtonIndex:(NSInterger)buttonIndex:
//当用户通过单击某个按钮完全隐藏该警告框时激发该方法。
-(void)alertViewCancel:(UIAlertView *)alertView:
//当该警告框被取消时（如用户单击了Home键）激发该方法。
```
    + 带输入框的UIAlertView
        - UIAlertView支持actionSheetStyle属性，用于设置该UIAlert的风格，支持如下枚举值：
        
        `UIAlertViewStyleDefault`：默认的警告框风格。
        `UIAlertViewStyleSecureTextInput`：警告框中包含一个密码输入框。
        `UIAlertViewStylePlainTextInput`：警告框中包含一个普通的输入框。
        `UIAlertViewStyleLoginAndPasswordInput`：警告框中包含用户名、密码两个输入框。
    + 实例代码
```Objective-C
-(void)viewDidLoad
{
    [super viewDidLoad];
}
-(IBAction)clicked:(id)sender
{
    UIAlertView *alert = [[UIAlertView alloc]
        initWithTitle:@"登录"
        message:@""
        delegate:self
        cancelButtonTitle:@"取消"
        otherButtonTitles:@"确定",nil];
    alert.alertViewStyle = UIAlertViewStyleLoginAndPasswordInput;
    [alert textFieldAtIndex:1].keyboardType = UIKeyboardTypeNumberPad;
    [alert show];
}
-(void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInterger)buttonIndex
{
    if(buttonIndex == 1){
        UITextField* nameField = [alertView textFieldAtIndex:0];
        UITextField* passField = [alertView textFieldAtIndex:1];
        NSString* msg = [NSString stringWithFormat:"输入的用户名为：%@，密码为%@",nameField.text,passField.text];
        UIAlertView *alert = [[UIAlertView alloc]
            initWithTitle:@"提示"
            message:msg
            delegate:nil
            cancelButtonTitle:@"确定"
            otherButtonTitles:nil];
        [alert show];
    }
}
-(void)willPresentAlerView:(UIAlertView *)alertView
{
    for(UIView * view in alertView.subViews)
    {
        if([view isKindOfClass[UILabel class])
        {
            UILabel* label = (UILabel*)view;
            label.textAlignment=UITextAlignmentLeft;
        }
    }
}
```

##UIActionSheet
- 显示在底部的按钮列表，用户通过单击某个按钮来表明自己的态度。
    + 风格(actionSheetStyle)：
       
    `UIActionSheetStyleDefault`:默认风格，灰色背景上显示白色文字。
    `UIActionSheetStyleBlackTranslucent`:在透明的黑色背景上显示白色文字。
    `UIActionSheetStyleBlackOpaque`:在纯黑的背景上显示白色文字
- 示例代码
```Objective-C
-(void)viewDidLoad
{
    [super viewDidLoad];
}
-(IBAction)clicked:(id)sender{
    UIActionSheet* sheet = [[UIActionSheet alloc] 
        initWithTitle:@"请确认是否确认"
        delegate:self
        cancelButton:@"取消"
        destrutiveButtonTitle:@"确定"
        otherButtonTitle:@"按钮一",@"按钮二",nil];
    sheet.actionSheetStyle = UIActionSheetStyleAutomaic;
    [sheet showInView:self.view];
}
-(void)actionSheet:(UIActionSheet *)actionSheetclickedButtonAtIndex:(NSInteger)buttonIndex
{
    UIAlertView* alert = [[UIAlertView alloc] initWithTitle:@"提示"
        message:[NSString stringWithFormat:@"你单击了第%d个按钮",buttonIndex
        delegate:nil
        cancelButtonTitle:@"确定"
        otherButtonTitles:nil];
    [alert show];
}
```