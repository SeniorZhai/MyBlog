title: ReactNative-真机运行
date: 2015-11-24 19:57:26
categories: React Native
tags: [样式,Style,Android,iOS]
---
<!--more-->
##声明
```js
var styles = StyleSheet.create({
	base: {
		width:38,
		height:38,
	},
	background:{
		backgroundColor:"#222222",
	},
})
```
##使用
```js
<Text style={styles.base} />
<View style={styles.background} />
<View style={[styles.base,style.background]} /> //可以接受多个style属性
```