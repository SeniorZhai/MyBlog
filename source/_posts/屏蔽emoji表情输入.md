title: 屏蔽emoji表情输入
date: 2015-01-22 11:33:09
categories: iOS
tags: [emoji]
---
很多时候我们的输入不需要emoji表情，TextFilld输入时先判断不反回给编辑框，即可拦截
<!--more-->
```objective-c
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString*)string{
   DLog(@"[[UITextInputMode currentInputMode]primaryLanguage] is %@",);
   if ([[[UITextInputMode currentInputMode]primaryLanguage] isEqualToString:@"emoji"]) {
       return NO;
   }
   return YES;
}
```