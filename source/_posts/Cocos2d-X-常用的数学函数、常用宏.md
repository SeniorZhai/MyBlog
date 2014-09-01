title: Cocos2d-X 常用的数学函数、常用宏
date: 2014-08-12 17:30:59
categories: Cocos2d-X
tags: [Cocos2d-X]
---
- `ccp(x,y)` 创建一个向量
- `ccpFromSize(s)` 以size s的width为x，height为y创建一个向量
- `ccpAdd(v1,v2)` 向量之间的加法
- `ccpSub(v1,v2)` 向量之间的减法
- `ccpNeg(v)` 向量取反
- `ccpMult(v,s)` 数乘，等价于ccp(v.x*s,v.y*s) s是一个浮点数
- `ccpMidpoint(v1,v2)` 取中点
- `ccpDot(v1,v2)` 点乘 等价于 v1.x*v2.x + v1.y*v2.y
- `ccpCross(v1,v2)` 叉乘 等价于 v1.x*v2.y - v1.y*v2.x
- `ccpProject(v1,v2)` 返回向量v1在向量v2的投影向量
- `ccpLength(v)` 返回向量v的长度
- `ccpLengthSQ(v)` 返回向量v的长度的平方
- `ccpDistance(v1,v2)` 返回点v1到v2的距离
- `cppDistanceSQ(v1,v2)` 返回点v1到v2距离的平方
- `ccpNormalize(v)`  返回v的标准化向量，就是长度为1  

- `ccpRotate(v1,v2)` 向量v1旋转过向量v2的角度并且乘上向量v2的长度。当v2是一个长度为1的标准向量时就是正常的旋转了，可以配套地用ccpForAngle  
- `ccpPerp(v)` 等价于 ccp(-v.y, v.x); （因为opengl坐标系是左下角为原点，所以向量v是逆时针旋转90度）  
- `ccpRPerp(v)` 等价于 ccp(v.y, -v.x); 顺时针旋转90度  

- `ccpForAngle(a)` 返回一个角度为弧度a的标准向量  
- `ccpToAngle(v)` 返回向量v的弧度
- `ccpAngle(a, b)` 返回a，b向量指示角度的差的弧度值 
- `ccpRotateByAngle(v, pivot, angle)` 返回向量v以pivot为旋转轴点，按逆时针方向旋转angle弧度 

- `CC_RADIANS_TO_DEGREES(a)` 弧度转角度
- `CC_DEGREES_TO_RADIANS(a)` 角度转弧度 
- `CCRANDOM_MINUS1_1()` 产生-1到1之间的随机浮点数
- `CCRANDOM_0_1()` 产生0到1之间的随机浮点数  

- `CCAssert(cond,msg)` 断言表达式cond为真，如果不为真，则显示字符串msg信息 

```c++
CCArray* _array;  
CCObject* _object;     // 用来遍历数组的临时变量  
CCARRAY_FOREACH(_array, _object) // 正向遍历  
{  
    // todo with _object....  
}  
  
CCARRAY_FOREACH_REVERSE(_array, _object) // 反向遍历  
{  
    // todo with _object....  
}  
CCDictionary* _dict;  
CCDictElement* _elmt; // 遍历表的临时变量  
CCDICT_FOREACH(_dict, _elmt)  
{  
　　// todo with elmt;  
}  
```
- CREATE_FUNC() 创建构造函数
```c++
#define CREATE_FUNC(__TYPE__)
static __TYPE__* create()
{
	__TYPE__ *pRet = new __TYPE__();
	if (pRet && pRet->init())
	{
		pRet->autorelease();
		return pRet;
	}
	else 
	{
		delete pRet;
		pRet = NULL;
		return NULL;
	}
}
```