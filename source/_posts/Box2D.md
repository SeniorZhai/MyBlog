title: Box2D
date: 2014-08-19 11:31:33
categories: Cocos2d-X
tags: [Box2D,物理引擎]
---
一款功能强大、性能强劲的物理引擎。它能都维持在一个独立的运行空间。
## 概述
Box2D引擎创建的物体都属于理想的刚体，即它内部之间的距离永不改变。Box2D创建的物理世界具备以下三个特点。
1. 物理世界存在一个重力（可以为0）
2. 物理世界可以是一个存在边界的范围
3. 在世界中加入静态和动态物体（具有质量和位置），它们被用来模拟现实的运动规律
## 概念
1. 物理世界（World)
一个物理世界就是由各种刚体、框架、形状互相作用的集合。
2. 刚体(Rigid Bodies)
理想刚体是一个有限尺寸、可以忽略形变的物体。不论是否感受到作用力，在刚体内部，点与点之间的距离都不会改变，可以认为是一个连续质量分布体。 在Box2D物理世界中，所有物体都是刚体，这些物体一旦被创建出来后，其形状就不会改变。
3. 形状(Shapes)
2D平面几何结构，主要用于世界里的物理碰撞的检测。
4. 框架(Fixtures)
框架代表物理世界中物体的固定物质形态，是连接物体和形状之间的框架，一个物体可以有多个框架，没个框架可以连接一个形状、它本身存储着物体的质量、密度、摩擦系数，同时包含碰撞标志、用户数据和是否使用碰撞检测器的标志。他存在的目的主要是为了物理引擎的运算考虑。
5. 关节(Joins)
关节的作用是将两个有一定运动自由度(水平、数组、转动)的物体连接起来，同时约束连接的物体。Box2D引擎支持的关节类型有旋转、棱柱、距离等等，通常具备两个共同属性：限制(Limits)和马达(Motors)
	(1) 关节限制(joint limit) 限定了一个关节的运动范围。
	(2) 关节马达(joint motor) 能依照关节的自由度来驱动所连接的物体。
## 模块
Box2D引擎由三个模块组成：公共(Common)、碰撞(Collision)和动态(Dynamics)。
(1) Common核心模块包含了内存分配、数学计算和配置
(2) Collision模块定义了形状(shape)、broad-phase检测盒碰撞功能/查询(collision functions/queries)
(3) Dynamics模块提供对世界(world)、刚体(bodies)、框架(fixtures)和关节(joint)模拟
## 数据单位
Box2D引擎使用浮点数进行计算，所以为看保证它的正常工作，必须使用一些公差来保证它的正常工作。引擎能够良好的处理0.1~10米之间的移动物体。
## 用户数据
```c++
// 创建一个精灵
CCSprite* actor = CCSprite::node();
// 物体的定义
b2BodyDef bodyDef;
// 指定数据
bodyDef.userData = actor;
// 创建物体
actor->body = box2Dworld->CreateBody(&bodyDef);
```
用户数据是可选的，并且能放入任何东西，为了保持一致性，通常保存精灵对象的指针。

## 物理世界World
创建一个世界
```c++
b2Vec2 gravity;	// 重力
gravity.Set(0.0f,-10.0f); 
world = new b2World(gravity);
world->SetAllowSleeping(true);	// 设置允许物体进入休眠状态
world->SetContinuonsPhysics(true); // 设置使用连续物理检测
```
销毁世界
```c++
delete world;
world = NULL;
```
## 世界运转起来
物理世界类中用于驱动模拟的函数为`Step`，调用它时，需要指定一个时间步和一个速度及位置的迭代次数。
```c++
float32 timeStep = 1.0f/60.0f;	// 时间步，频率
int32 velocityIterations = 10;	// 速度迭代
int32 positionIterations = 8;	// 位置迭代次数
myWorld->Step(timeStep,velocityIterations,positionIterations);	// 驱动函数
``` 
在物理引擎执行一次时间步之后，最好是清除任何施加在物体上的力。可以使用`b2World::ClearForces`完成，可以去除力的持续作用效果。
## 探索世界
物理世界中存在创建工厂，工厂则会创建物体、框架和关节对象，同时世界也是容器，它会容纳工厂创建出来的所有对象。可以通过遍历的方式来获取世界中所有物体、框架和关节。
```c++
for (b2Body* b = myWorld()->GetBodyList();b;b = b->GetNext())
{
	b->WakeUp();	// 唤醒物体
}
```
## AABB查询
Box2D引擎当中提供了AABB查询的方式，需要得出一个区域内所有对象时的解决方法。为此专门使用`broad-phase`的数据结构，它提供了一个log(N)复杂度的快速查询方法。
```c++
class MyQueryCallback : public b2QueryCallback
{
public:
	bool ReportFixture(b2Fixture* fixture)
	{
		b2Body* body = fixture->GetBody();
		body->WakeUp();
		return ture; // 继续查询
	}
}

b2AABB aabb;
aabb.lowerBound.Set(-1.0f,-1.0f);	// 设置AABB的上边界
aabb.upperBound.Set(1.0f,1.0f);		// 设置AABB的下边界
myWorld->Query(&callback,aabb);
```
## 光线投射(Ray Casts)
```c++
class MyRayCastCallBack : public b2RayCastCallback
{
public:
	MyRayCastCallback()
	{
		m_fixture = NULL;
	}
	float32 ReportFixture(b2Fixture* fixture,const b2Vec2& point,const b2Vec2& normal,float32 fraction)
	{
		// 保存框架
		m_fixture = fixture;
		// 保存交点
		m_point = point;
		// 保存法向量
		m_normal = normal;
		// 保存光线通过的分数距离
		m_fraction = fraction;
		return fraction;
	}
	b2xFixture* m_fixture;
	b2Vec2 m_point;
	b2Vec2 m_normal;
	float32 m_fraction;
};
// 回调函数
MyRayCastCallback callback;
// 光线投射的起始点
b2Vec2 point(-1.0f,0.0f);
// 光线投射的结束点
b2Vec2 point(3.0f,1.0f);
// 开始光线的投射
myWorld->RayCast(&callback,point1,point2);
```
## 形状Shapes
形状是物体的包围盒，描述了可相互碰撞的几何对象的外形，它通常会是一个几何图形。被Shape是形状的基类，Box2D引擎中各种形状类都是继承自这个基类。此基类定义了几个常用函数：
1. 判断一个点与形状是否有重叠
2. 在形状上执行光线投射
3. 计算形状的AABB
4. 计算形状的质量
所有形状都有两个成员变量：类型(type)和半径(radius)，引擎存在两种类型的形状：圆形和多边形。
### 圆形(Circle Shapes)
圆形形状(b2CircleShape)是一个由位置和半径表示的集合图形。圆形形状都是实心的，主要的参数就是半径。
```c++
b2CircleShape circle;// 创建圆形形状的对象
circle.m_p.Set(1.0f,2.0f,3.0f);// 设置位置
circle.m_radius = 0.5f;// 设置半径
```
### 多边形(Polygon Shape)
多边形(b2PolygonShape)是实现的凸(Convex)多边形。数学的定义：在多边形内部任意选择两点，作一线段，如果所有线段都跟多边形的边不想交，这个多边形就是凸多边形。在Box2D引擎中一个多边形形状是通过顶点来描述的。
```c++
b2PolygonDef triangleDef;
triangleDef.vertexCont = 3;
triangleDef.vertices[0].Set(-1.0f,0.0f);
triangleDef.vertices[1].Set(1.0f,0.0f);
triangleDef.vertices[2].Set(0.0f,2.0f);
```
在创建多边形时，顶点必须是逆时针排列。创建多边形时，顶点的数据可以通过数组来传递，数组中的顶点数最大值是核心模块中定义的b2maxPolygonVertices，默认情况下此数值是8。
```c++
b2Vec2 vertices[4];
vertices[0].Set(0.0f,0.0f);
vertices[1].Set(1.0f,0.0f);
vertices[2].Set(0.0f,1.0f);
vertices[3].Set(1.0f,1.0f);

int32 count = 4;
b2PolygonShape polygon;
polygon.Set(vertices,count);
```
为了使用方便引擎还提供了一些定义好的初始化函数来创建固定的多边形形状：箱子(box)和边缘(edge，就是线段)
```c++
void SetAsbox(float32 hx,float32 hy);
void SetAsBox(float32 hx,float32 hy,const b2Vec2& center,float32 angle);
void SetAsEdge(const b2Vec2& v1,const b2Vec& v2);
```
## 框架Fixtures
Fixture是动态模块中重要的概念，用来物理模拟。
动态模块包含的内容：
- 框架(Fixtures)
- 物体(Bodies)
- 接触(Contacts)
- 关节(Joints)
- 世界(World)
- 监听器(Listener)
框架主要用于建立形状与物体之间的连接和进行物理模拟。
### 密度(Density)
框架的密度用来计算物体的质量属性，密度可以为零或者其他整数。
### 摩擦(Friction)
Box2D支持静态摩擦和动态摩擦，两者都使用的相同的摩擦系数作为参数。摩擦的强度与正交力成正比。摩擦系数经常会设置为0到1之间，0意味着没有摩擦，1会产生强摩擦。当计算两个形状之间的摩擦时，引擎必须计算两个形状的摩擦参数。
```c++
float32 friction;
friction = sqrtf(shape1->friction * shape2->friction);
```
### 恢复(Resitution)
为反弹的参数，它可以使对象弹起。恢复的值通常设置在0到1之间。恢复就是一个物体碰撞后反弹大小的参数，其值为0表示小球不会弹起，这称为非弹性碰撞。值为1表示小球的速度大小一样，只是方向相反，这称为完全弹性碰撞。
```c++
float32 resitution;
restitution = b2Max(shape1->resitution,shape2->resitution);
```
### 帅选(Filtering)
用来设置某些游戏对象相互碰撞的关系。Box2D支持最多十六个种群进行筛选。在引擎中碰撞筛选是通过掩码来完成的。
```c++
playerFixtureDef.filter.categoryBits = 0x0002;
playerFixtureDef.filter.maskBits = 0x0004;
monsterFixturDef.filter.categoryBits = 0x0004;
monsterFixturDef.filter.maskBits = 0x0002;
```
还可以进行分组，可以让同一组内的所有框架总是互相碰撞(正索引)或者永不碰撞(负索引)
```c++
fixture1Def.filter.groupIndex = 2;
fixture2Def.filter.groupIndex = 2;
fixture3Def.filter.groupIndex = -8;
fixture4Def.filter.groupIndex = -8;
```
1. 静态物体上的框架永远不会与两一个静态物体上的框架发生碰撞，因为静态物体根本不会移动
2. 同一个物体上的框架永远不会相互碰撞
3. 如果物体用关节连接起来了，物体上的框架可以选择启用或禁止它们之间相互碰撞。
### 感应器(Sensors)
感应器是用来检测两个框架是否相交的对象。可以将任何一个框架对象设置为感应器，感应器可以是静态或者动态的。感应器发生碰撞时，是不会生成接触点的。
```c++
b2Contact::IsTouching	// 这个函数可以得到感应器是否接触
b2ContactListener::BeginContact和EndContact // 开始接触点和结束接触点
```
## 物体Bodies
Box2D的物体均为刚体，它具有质量和惯性，运动状态由重心和受力决定，动态速度由线速度和角速度控制，可分为三类
1. 静态物体(b2_staticBody)质量为0，在模拟时，静态不可移动，不过可以手动设置移动。速度为零。另外它不会和其他静态、平台物体碰撞。
2. 平台物体(b2_kinematicBody)
是按照固定轨迹在运动的物体，其质量也为零，可以预设一个速度，它也不会和其他的静态或平台物体相互碰撞。
3. 动态物体(b2_dynamicBody)
具有质量、速度、摩擦等，它将会参与引擎中的碰撞检测与物理模拟。可以与物理世界中任何对象发生碰撞。
上述物体都可以被施加外力(forces)、扭矩(torques)、冲量(impulses)
### 位置和角度(Position and Angle)
是物体的最基本的属性，也就是物理世界都不会缺少这两个属性。
物体的两个主要有两个属性是最最主要的：
1. 第一个是物体的原点
2. 第二个是物体的知心，可以显式地通过`b2MassData`来设置。
```c++
bodyDef.position.Set(0.0f,2.0f); 	// body的原点
bodyDef.angle = 0.25f * b2_pi;		// 弧度制下body的角
```
在引擎运转过程中，物体的位置和角度也会随时地改变。也可以通过物体类中的函数来改变。
```c++
void SetTransform(const b2Vec2& position,float32 angle);
```
### 阻尼(Damping)
用于表示减小物体的世界中的速度的数值。阻尼和摩擦有所不同，摩擦仅在物体有接触的时候才会发生，而阻尼却时时刻刻阻碍着物体运动，比如空气阻力。阻尼参数的数值范围可以是从零到无穷大。通常来说阻尼的值应该在0~0.1之间。
```c++
bodyDef.linearDamping = 0.0f;
bodyDef.angularDamping = 0.01f;
```
### 休眠参数(Sleep Parameters)
确定一个物体已经停止了移动时，物体就会进入休眠状态，休眠物体只消耗很小的CPU开销。接触到一个醒着的物体，物体就会被唤醒，物体上艹关节或触点被销毁的时候，也会醒来。
```c++
bodyDef.allowSleep = true;	// 可以休眠
bodyDef.awake = ture;	// 休眠物体
```
### 固定旋转(Fixed Rotation)
通过`bodyDef.fixedRotation = ture`物体的转动惯量设置成零，物体会保持初始化的角度，而不会发生旋转。
### 子弹(Bullets)
子弹通常指一类速度很快而尺寸较小的物体，容易击穿其他物体。`bodyDef.bullet = true`
### 活动状态(Activation)
一个非活动状态的物体，将完全地与世隔绝，不会参与物理模拟和碰撞检测，关节也不会被迷你。`bodyDef.active = true`
### 用户数据(User Data)
一个存放用户数据的void指针，通常保存精灵对象
```c++
b2BodyDef bodyDef;
bodyDef.userData = &myActor;
```
## 关节(Joints)
关节是将两个或两个以上物体连接在一起的对象，主要有三个作用
1. 用于连接物体
2. 用于限制物体的移动，是一种约束
3. 它本身是一个能够活动的对象
### 关节的定义(JointDef)
关节包含一个用户数据的void指针，可以指定游戏元素，希望关节上的两个物体也能够发生碰撞，可以设置`collideConnected`布尔值来允许连接的物体之间进行碰撞。