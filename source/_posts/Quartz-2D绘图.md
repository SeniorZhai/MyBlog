title: Quartz 2D绘图
date: 2014-02-26 12:43:53
categories: iOS
tags: [iOS]
---
- 使用Quartz 2D绘图的关键步骤有两步：获取CGContextRef，调用CGContextRef的方法进行绘图。
1. 自定义UIView获取CGContextRef
```Object-C
CGContextRef ctx = UIGraphicsGetCurrentContext();
```
2. 创建位图获取CGContextRef 
>
```Object-C
// 创建内存中的图片
UIGraphicsBeginImageContext(CGSizeMake(320,480));
// 获取想内存中图片执行绘制的CGContextRef
CGContextRef ctx = UIGraphicsGetCurrentContext();
```

## Quart2D的绘图相关函数

|**函数签名**|**简要说明**|
|---|:---|
|void CGContextClearRect(CGContextRef c,CGRect rect)|擦除指定矩形区域上绘制的图形|
|void CGContextDrawPath(CGContextRef c,CGPathDrawingMode mode)|使用指定模式绘制当前CGContextRef中所包含的路径。第二个参数支持kCGPathFill、kCGPathEOFill、kCGPathStroke、kCGPathFillStroke、kCGPathEOFillStroke|
|void CGContextEOFillPath(CGContextRef c)|用奇偶规则来填充该路径包围的区域，奇偶规则指：如果某个点被路径包围了奇数次，系统绘制该点：如果被路径包围偶数次，系统不绘制该点|
|void CGContextFillPath(CGContextRef c)|填充该路径包围的区域|
|void CGContextFillPath(CGContextRef c,CGRect rect)|填充rect代表的矩形|
|void CGContextFillRects(CGContextRef c,const CGRect rects[],size_t_count)|填充多个矩形|
|void CGContextFillElipseInRect(CGContextRef context,CGRect rect)|填充rect矩形的内切椭圆|
|void CGContextStrokePath(CGContextRef c)|使用当前CGContextRef设置的线宽绘制路径|
|void CGContextStrokeRect(CGContextRef c,CGRect rect)|使用当前CGContextRef设置的线宽绘制矩型|
|void CGContextStrokeRectWillWidth(CGContextRef c,CGRect rect,CGFloat width)|使用指定线宽绘制矩形框|
|void CGContextReplacePathWillStrokePath(CGContextRef c)|使用绘制当前路径时覆盖的区域作为当前CGContextRef中的新路径|
|void CGContextStokrEllipseInRect(CGContextRef context,CGRect rect)|使用当前CGContextRef设置的线宽绘制rect矩形的内切椭圆|
|void CGContextStrokeLineSegments(CGContextRef c,const CGPoint points[],size_t count)|使用当前CGContextRef设置的线宽绘制多条线段，其中1、2点组成一个线段，3、4组成一条限度，以此类推|

## 设置绘图属性的相关函数
|**函数签名**|**简要说明**|
|---|:---|
|void CGContextSavaGState(CGContextRef c);|保存CGContextRef当前的绘图状态，以便以后恢复该状态|
|void CGContextRestoreGState(CGContextRef c);|把CGContextRef的状态恢复到最近一次保存时的状态|
|CGInterpolationQuality CGContextGetInterpolationQuality(CGContextRef c);|获取当前CGContextRef在放大图片时差值质量|
|void CGContextSetInterpolationQuality(CGContextRef c,CGInterpolationQuality quality);|设置当前CGContextRef在放大图片时差值质量|
|void CGContextSetLineCap(CGContextRef c,CGLineCap cap);|设置线段端点的绘制形状，该属性支持如下三个值：kCGLineCapButt：该属性不会只端点，线条结尾处直接结束，此为默认值；kCGLineCapRound：该属性指定绘制原点端点，线条结尾处绘制一个直径为线条宽度的半圆；kCGLineCapSquare:该属性指定绘制方形端点。线条结尾处绘制半个边长为线条宽度的正方形。|
|void CGContextSetLineDash(CGContextRef c,CGFloat phase,const CGFloat lengths[],size_t count);|设置绘制边框时所用的点线模式|
|void CGContextSetLineJoin(CGContextRef c,CGLineJoin join);|设置线条连接点的风格支持以下三个值：kCGLineJoinMeter,kCGLineJoinRound,kCGLineJoinBevel|
|void CGContextSetLineWidth(CGContextRef c,CGFloat width);|设置绘制直线、边框时的线条宽度|
|void CGContextSetMiterLimit(CGContextRef c,CGFloat limit);|当把连接点风格设置为meter风格时，该方法用于控制锐角箭头的长度|
|void CGContextSetPatternPhase(CGContextRef c,CGSize phase);|设置该CGContextRef采用位图填充的相位|
|void CGContextSetFillPattern(CGContextRef c,CGPatternRef pattern,const CGFloat components[]);|设置该CGContextRef使用位图填充|
|void CGContextSetShouldAntialias(CGContextRef c,bool shouldAnitialias);|设置该CGContextRef是否应该抗锯齿|
|void CGContextSetStrokePattern(CGContextRef c,CGPatternRef pattern,const CGFloat components[]);|设置该CGContextRef使用位图绘制线条、边框|
|void CGContextSetBlendMode(CGContextRef context,CGBlendMode mode);|设置CGContextRef的叠加模式|
|void CGContextSetAllowsAntialiasing(CGContext context,bool allowsAntialiasing);|设置该CGContextRef是否允许抗锯齿|
|void CGContextSetAllowsFontSmoothing(CGContext context,bool allowsFontSmoothing);|设置该CGcontextRef是否允许光滑字体|
|void CGContextSetShouldSmoothFonts(CGContext c,bool shouldSmoothFonts);|设置该CGcontextRef是否允许光滑字体|
|void CGContextSetAlpha(CGContext c,CGFloat alpha);|设置全局透明度|
|void CGContextSetCMKYFillColor(CGContextRef c,CGFloat cyan,CGFloat magenta,CGFloat yellow,CGFloat black,CGFloat alpha);|使用CMYK颜色模式来设置该CGContextRef的填充颜色|
|void CGContextSetCMYKStrokeColor(CGContextRef c,CGFloat cyan,CGFloat magenta,CGFloat yellow,CGFloat black,CGFloat alpha)|使用CMYK颜色模式来设置该CGContextRef的线条颜色|
|void CGContextSetFillColorWithColor(CGContextRef c,CGColorRef color);|使用指定颜色来设置该CGContextRef的填充颜色|
|void CGContextSetStrokeColorWithColor(CGContextRef c,CGColorRef color);|使用指定颜色来设置该CGContextRef的线条颜色
|void CGContextSetGrayFillColor(CGContextRef c,CGFloat gray,CGFloat alpha);|使用灰色来设置该CGContextRef的填充颜色|
|void CGContextSetGrayStrokeColor(CGContextRef c,CGFloat gray,CGFloat alpha);|使用灰色来设置该CGContextRef的线条颜色|
|void CGContextSetRGBFillColor(CGContextRef c,CGFloat red,CGFloat green,CGFloat blue,CGFloat alpha);|使用RGB颜色模式来设置该CGContextRef的填充颜色|
|void CGContextSetRGBStoreColor(CGContextRef c,CGFloat red,CGFloat green,CGFloat blue,CGFloat alpha);|使用GRB颜色模式来设置该CGContextRef的线条颜色|
|void CGContextSetShadow(CGContext context,CGSize offset,CGFloat blur);|设置阴影在X,Y方向上的偏移，以及模糊度（blur值越大，阴影越模糊），默认模糊颜色为RGBA(0,0,0,1/3.0)|
|void CGContextSetShadowWithColor(CGContextRef context,CGSize offset,CGFloat blur,CGColorRef color);|设置阴影在X、Y方向上的偏移，以及模糊度和阴影颜色|

## 点线模式
- Quartz 2D绘制线段或边框时，默认总是使用实线，使用点线进行绘制，可调用CGContextRefLineDash(CGContextRef c,CGFloat phase,const CGFloat lengths[],size_t count)进行设置。

## 绘制文本
- CGContextRef为绘制文字提供了如下函数
	+ `CGAffineTransform CGContextGetTextMartix(CGContextRef c):`：获取当前对文本执行变换的变换矩阵。
	+ `CGPoint CGContextGetTextPosition(CGContextRef c):`：获取该CGContextRef中当前绘制文本的位置。
	+ `CGContextSelectFont(CGContextRef c,const char *name,CGFloat size,CGTextEncoding textEncoding):`：设置该CGContextRef当前绘制文本的字体、字体大小。
	+ `void CGContextSetCharacterSpacing(CGContextRef c,CGFloat spacing):`：设置该CGContextRef中绘制文本的字符间距。
	+ `void CGContextSetFont(CGContextRef c,CGFonRef font):`：设置该CGContextRef中绘制文本的字体。
	+ `void CGContextSetFontSize(CGContextRef c,CGFloat size):`：设置该CGContextRef中绘制文本的字体大小。
	+ `CGContextSetTextDrawingMode(CGContextRef c,CGTextDrawingMode mode):`：设置该CGContextRef绘制文本的绘制模式。该函数支持kCGTextFill、kCGTextStroke、kCGTextFillStroke等绘制模式。
	+ `void CGContextSetTextMatrix(CGContextRef c,CGAffineTransform t):`：设置对将要绘制的文本执行指定的变换。
	+ `void CGContextSetTextPosition(CGContextRef c,CGFloat x,CGFloat y):`：设置CGContextRef的一个文本绘制位置。
	+ `void CGContextShowText(CGContextRef c,const char* string,size_t length):`：控制CGContextRef在当前绘制点绘制指定文本。
	+ `void CGContextShowTextAtPoint(CGContextRef c,CGFloat x,CGFloat y,const char *string,size_t length):`：控制CGContextRef在指定绘制点绘制文本。
- 使用CGContextRef绘制文本的步骤：
1. 获取绘图的CGContextRef
2. 设置绘制文本的相关属性
3. 如果只是绘制不需要进行变换的文本，直接调用NSString的drawAtPoint:withAttributes:、drawInAttrubutes:withFont:等方法绘制即可。如果需要绘制文本进行变换，则需要先调用CGContextSetTextMatrix()函数设置变换矩阵，再调用CGContextShowTextAtPoint()方法绘制文本。

## 使用路径
-绘制复杂的图形，需要使用路径
- 创建路径的相关函数
|**函数签名**|**简要说明**|
|---|:---|
|void CGContextBeginPath(CGContextRef c);|开始定义路径|
|void CGContextClosePath(CGContextRef c);|关闭前面定义的路径|
|void CGContextAddArc(CGContextRef c,CGFloat x,CGFloat y,CGFloat radius,CGFloat startAngle,CGFloat endAngle,int clockwise);|向CGContextRef的当前路径添加一段弧|
|void CGContextAddArcToPoint(CGContextRef c,CGFloat x1,CGFloat y1,CGFloat x2,CGFloat y2,CGFloat radius);|向CGContextRef的当前路径添加一段贝塞尔曲线|
|void CGContextAddCurveToPoint(CGContextRef c,CGFloat cp1x,CGFloat cp1y,CGFloat cp2x,CGFloat cp2y,CGFloat x,CGFloat y);|向CGContextRef的当前路径添加一段贝塞尔曲线|
|void CGContextAddLines(CGContextRef c,const CGPoint points[],size_t count);|向CGContextRef的当前路径上添加多条线段。该方法需要传入N个CGPoint组成的数组，其中1、2点组成第一条线段，2、3点组成一条线段……|
|void CGContextAddLineToPoint(CGContextRef c,CGFloat x,CGFloat y);|把CGContextRef的当前路径从当前结束点连接到x、y对应的点|
|void CGContextAddQuadCurveToPoint(CGContextRef c,CGFloat cpx,CGFloat cpy,CGFloat x,CGFloat y);|向CGContextRef的当前路径从当前结束点连接到x、y对应的点|
|void CGContextAddRect(CGContextRef c,CGRect rect);|向CGContextRef的当前路径添加一个矩形|
|void CGContextAddRects(CGContextRef c,const CGRect rects[],size_t count);|向CGContextRef的当前路径添加多个矩形|
|void CGContextMoveToPoint(CGContextRef c,CGFloat x,CGFloat y);|向CGContextRef的当前结束点移动到x、y对应的点|
|void GCContextAddEllipseInRect(CGContextRef c,CGRect rect);|向CGContextRef的当前路径添加一个椭圆|
|CGPathRef CGContextCopyPath(CGContextRef context);|复制当前CGContextRef包含的路径，该函数返回的CGPathRef代表当前CGContextRef包含的路径|
|void CGContextAddPath(CGContextref context,CGPathRef path);|将已有的CGPathRef代表的路径添加到当前CGContextRef的路径中|
- 获取CGContextRef所包含的路径信息的函数
	+ `bool CGContextIsPathEmpty(CGContextRef c):`:该函数用于判断指定CGContextRef包含的路径是否为空。
	+ `CGPoint CGContextGetPathCurrentPoint(CGContextRef c):`：该函数用于返回指定CGContextRef包含的路径的当前点。
	+ `CGRect CGContextGetPathBoundingBox(CGContextRef c):`：该函数用于返回指定CGContextRef中能完整包围所有路径的最小矩形。
	+ `bool CGContextPathContainsPoint(CGContextRef context,CGPoint point,CGPathDrawingMode mode):`：该函数判断指定CGContextRef包含的路径按指定绘制模式进行绘制时，是否需要绘制point点。
- 使用路径绘制步骤
1. 调用CGContextBeginPath()函数开始定义路径
2. 添加子路径
3. 调用CGContextClosePath()函数关闭路径
4. 调用CGContextDrawPath()、CGContextEOFFillPath()、CGContextFillPath()、CGContextStrokePath()函数来填充路径或绘制路径边框。
	+ CGContextDrawPath() 可以代替其他三个方法，它可以使用特定的模式绘制图形
		- kCGPathFill:指定填充路径
		- kCGPathEOFill:指定采用even-odd模式填充路径
		- kCGPathStroke:指定只绘制路径
		- kCGPathFillStroke:指定既绘制路径，也填充路径
		- kCGPathEOFillStroke:指定既绘制路径，也采用even-odd模式填充路径

##  Core Image滤镜
- IOS5新增框架，可以对图像进行各种特效处理，包括进行色彩调节、降噪、扭曲等。
- Core Image的三个核心API：
	+ CIContext：处理的核心API，所有图片的处理都在它的管理下完成。
	+ CIFilter：过滤器，所有的过滤器都由该CIFilter代表，在创建CIFilter时，需要传入不同的参数即可创建不同类型的过滤器。
	+ CIImage：被处理的图片，可通过UIImage、图片文件或像素数据来创建CIImage。
- 使用CoreImage的步骤
1. 创建CIContext对象
	+ 创建基于CPU的CIContext对象
	```Objective-C
	ctx = [CIContext contextWithOptions:[NSDicationary dictionaryWithObject:[NSNumber numberWithBool:YES] forKey:kCIContextUseSoftwareRenderer]];
	```
	+ 创建基于GPU的CIContext对象
	```Objective-C
	ctx = [CIContext contextWithOptions:nil]
	```
	+ 创建基于OpenGL优化CIContext对象，可获得实时性能
	```Objective-C
	EAGLContext * eaglctx = [[EAGLContext alloc] initWithAPI:kEAGLRenderingAPIOpenGLES2];
	ctx = [CIContext contextWithEAGLContext:eaglctx];
	```
2. 创建CIFilter过滤器，CIFilter提供了filterWithName：类方法来创建CIFilter对象。该方法需要传入过滤器的名字，根据不同名字创建不同的过滤器，可在在[此处](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html)查看。
3. 创建CIImage对象，该CIImage将要作为过滤器处理的源图片。
4. 调用CIFilter的[filter setValue:behinImage for:@"inputImage"]方法为inputImage属性赋值，该属性用于指定该过滤器将要处理的源图片。
5. 根据需要，为不同的过滤器设置不同的过滤参数。
6. 调用CIFilter的outputImage属性获取该过滤器处理后的图片，程序返回的是CIImage对象。
7. 调用CIContext的不同方法将CIImage转换成CGImageRef或将CImage绘制到指定区域中。
