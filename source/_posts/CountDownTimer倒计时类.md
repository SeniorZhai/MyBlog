title: CountDownTimer倒计时类
date: 2014-09-30 09:38:22
categories: Android
tags: [倒计时,Api]
---
Android官方有提供`CountDownTimer`类可以帮助我们很好的实现倒计时功能
<!--more-->
![](/img/14093001.gif)
```java
mButton.setOnClickListener(new View.onClickListener(){
	@Override
	public void onClick(View v){
		new CountDownTimer(10000,1000) {
			// 倒计时长，和回调间隔
			public void onTick(long millisUntilFinished) {
				mButton.setText(millisUntilFinished / 1000 + "s后重新发送");
			}
			public void onFinish() {
				mButton.setText("重新获取验证码");
				mButton.setEnabled(true);
			}
		}
	}
});
```