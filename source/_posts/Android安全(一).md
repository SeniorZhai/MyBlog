title: Android安全
date: 2014-03-03 19:15:47
categories: Android
tags: [Android]
---
## Android安全概述
- 安全主要解决4类需求：
	+ 保密(Security/Confidentiality
	+ 鉴别/认证(Authentication)
	+ 完整性(Integrity)
	+ 不可否认性(non-repudiation)
- 好的密码术应该是**算法公开**，**密钥保密**的。

### 对称加密概述
- 典型的加密模型
    + 秘钥：分为加密秘钥和解密秘钥。
    + 明文：文友进行机密，能够直接表带原文含义的信息。
    + 密文：经过加密处理处理之后，隐藏原文含义的信息。
    + 加密：将明文转换成密文的实施过程。
    + 解密：将密文转换成明文的实施过程。
![](https://github.com/zt1991616/blog/raw/master/Image/14030201.png)
- 对称的含义（Symmetric）
    + 加密和解密用同一套Key
- 置换加密、转置加密和乘积加密
    + 置换加密,又称换位密码,是一个简单的换位，每个置换都可以用一个置换矩阵Ek来表示。每个置换都有一个与之对应的逆置换Dk。
- DES
- AES
- 优点和缺点
    + 高效
    + 秘钥交换的问题
    + 不如RSA的加密安全程度高，但当选择256bit的AES仍然能胜任大多数安全领域。

### 非对称加密
- 非对称加密的模型
![](https://github.com/zt1991616/blog/raw/master/Image/14030202.png)
- 公钥和私钥
- RSA
	+ 建立在分解大树的困难度上，公钥/私钥长度至少1024bit
- 优点和缺点
    + 安全性足够高
    + 没有密钥交换的问题
    + 效率低，对于大数据加密很慢
### 常见场景
- 保密会话
![](https://github.com/zt1991616/blog/raw/master/Image/14030203.png)
