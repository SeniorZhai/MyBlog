title: Sprite精灵类
date: 2014-08-18 11:38:05
categories: Cocos2d-X
tags: [精灵,Sprite]
---
常用创建方法
```c++
bool MyScene::init()
{
	Size size = Director::getInstance()->getWinSize();

	Sprite *sp1 = Sprite::create("icon.png");
	sp1->setPosition(Vec2(size.with*0.2,size.height*0.7));
	this->addChild(sp1);

	Sprite *sp2 = Sprite::create("icon.png",Rect(10,30,28,28));
	sp2->setPosition(Vec2(size.width*0.4,size.height*0.7));
	this->addChild(sp2);
	// 创建纹理
	Texture2D *texture = TextureCache::sharedTextureCache()->addImage("icon.png");

	Sprite *sp3 = Sprite::createWithTexture(texture);
	sp3->setPosition(Vec2(size.width*0.6,size.height*0.7));
	this.addChild(sp3);

	Sprite *sp4 = Sprite::createWithTexture(texture,Rect(0,0,40,40));
	sp4->setPosition(Vec2(size.width*0.8,size.height*0.7));
	this.addChild(sp4);

	SpriteFrame *frame = SpriteFrame::create("icon.png",Rect(0,0,57,57));
	Sprite *sp5 = Sprite::createWithSpriteFrame(frame);
	sp5->setPosition(Vec2(size.width*0.3,size.height*0.3));
	this.addChild(sp5);

	Sprite *sp6 = Sprite::createWithSpriteFrame("icon.png");
	sp6->setPosition(Vec2(size.width*0.3,size.height*0.3));
    this->addChild(sp6);
     
    return true;
}
```
常用的类方法：
- setScale(float fScale) 缩放
- setRotation(float fRotation) 旋转
- setSkew(float s) 倾斜
- setAnchorPoint(const Point& another) 设置锚点
- setVisible(bool bvisible) 是否可见
- setColor(const cccolor3B& color3) 设置颜色
- setOpacity(Glubvte Opacity) 透明度设置 0-255 0表示完全透明，255表示不透明
- setTexture(CCTexture2D *texture) 更改图片