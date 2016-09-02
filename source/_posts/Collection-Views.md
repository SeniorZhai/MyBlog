title: Collection Views
date: 2014-03-21 12:59:00
categories: iOS
tags: [iOS]
---

![](https://github.com/zt1991616/blog/raw/master/Image/14032101.png)

## 什么是UICollectionView
UICollectionView是一种新的数据展示方式，简单来说可以把他理解成多列的UITableView。比如iBooks，一个虚拟的书架上放着各类图书，排列整齐，亦或者iPad的iOS6中的原生时钟中的各个时钟，也是UICollectionView的最简单的一个布局。

![](https://github.com/zt1991616/blog/raw/master/Image/14032102.png)

最简单的UICollectionView就是一个GirdView，可以以多列的方式来将数据进行展示，标准的UICollection包含三个部分：
- Cells用于展示内容的主体，对于不同的cell可以指定不同尺寸和不同的内容
- Supplementary Views追加视图，用来标记每个section的view
- Decoration Views装饰视图，每个section的背景

![](https://github.com/zt1991616/blog/raw/master/Image/14032103.png)
![](https://github.com/zt1991616/blog/raw/master/Image/14032104.png)

## 实现一个简单的UICollectionView

和UITableView一样，UICollectionView同样采用datasource和delegate设计模式：datasource为View提供数据源，告诉View要实现什么及如何显示它们，delegate提供一些样式的小细节以及用户交互的相应。

### UICollectionViewDataSource
- - (NSInteger)collectionView:(UICollectionView *)view numberOfItemsInSection:(NSInteger)section:section的数量
- - (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section:某个section里有多少个item
- - (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath:指定显示什么样的cell

实现了以上三个委托方法，基本上就可以保证CollectionView工作正常，同时还通过了Supplementary View方法
- - (UICollectionReusableView *)collectionView:(UICollectionView *)collectionView viewForSupplementaryElementOfKind:(NSString *)kind atIndexPath:(NSIndexPath *)indexPath

#### 用于重用

在UICollectionView中在请求数据源之前要注册CellView、SupplementaryView，可以通过以下方法进行注册
-registerClass:forCellWithReuseIdentifier:
-registerClass:forSupplementaryViewOfKind:withReuseIdentifier:
-registerNib:forCellWithReuseIdentifier:
-registerNib:forSupplementaryViewOfKind:withReuseIdentifier:

### UICollectionViewDelegate
负责交互
- cell的高亮
- cell的选中状态
- 可以支持长按后的菜单
用户点击cell的时候，现在会按照以下流程向delegate进行询问：
1. - collectionView:shouldHighlightItemAtIndexPath:是否应该高亮？
2. - collectionView:didHighlightItemAtIndexPath:如果1回答是，那么高亮
3. - collectionView:shouldSelectItemAtIndexPath:无论1结果如何，都询问是否可以被选中？
4. - collectionView:didUnhighlightItemAtIndexPath:如果1回答是否，那么取消高亮
5. - collectionView:didSelectItemAtIndexPath:如果3回答为是，那么选中cell

对应的高亮和选中状态分别由highlighted和selected两个属性表示。

### 关于cell

UICollectionViewCell相比UITableViewCell没有太多的花头，首先不存在格式各样的默认的style。UICollectionViewCell结构上相对比较简单，由下至上：
1. cell本身作为容器View
2. 一个大小自动适应整个cell的backgroundView，用于cell默认的背景
3. selectedBackgroundView，是cell被选中时的背景
4. contentView，自定义内容应被加载这个View上。

### UICollectionViewLayout
负责各个cell、Supplementary View和Decoration Views进行组织，为了它们设定各自的属性，包括但不限于：位置、尺寸、透明度、层级关系、形状、等等...

- Layout决定了UICollectionView是如何显示在界面上的。在展示之前，一般需要生成合适的UICollectionViewLayout子类对象，并将其赋予CollectionView的collectionViewLayout属性。

默认常用的Layout是UICollectionViewFlowLayout，是一个直线对齐的Layout。

- itemSize，定义了每一个item的大小。通过设定itemSize可以全局改变所有cell的尺寸，如果想要对某个cell定制尺寸，可以使用-collectionView:layout:sizeForItemAtIndexPath:方法。
- 间隔 指定每个item之间的间隔和每一行之间的间隔，和size类似，有全局属性，也可以对每一个item和每一个section做出设定：
	+ @property (CGSize) minimumInteritemSpacing
	+ @property (CGSize) minimumLineSpacing
	+ -collectionView:layout:minimumInteritemSpacingForSectionAtIndex:
	+ -collectionView:layout:minimumLineSpacingForSectionAtIndex:
- 滚动方向 由属性scrollDirection确定scroll View的方向，将影响Flow Layout的基本方向和由header及footer确定的section之间的宽度
	+ UICollectionViewScrollDirectionVertical
	+ UICollectionViewScrollDirectionHorizontal
- Header和Footer尺寸 分为全局和部分，需要注意根据滚动方向不同，header和footer的高和宽中只有一个和会起作用，垂直滚动时section间宽度为该迟钝的高，而水平滚动时为宽度起作用
	+ @property (CGSize) headerReferenceSize
	+ @property (CGSize) footerReferenceSize
	+ -collectionView:layout:referenceSizeForHeaderInSection:
	+ -collectionView:layout:referenceSizeForFooterInSection:
- 缩进
	+ @property UIEdgeInsets sectionInset;
	+ -collectionView:layout:insetForSectionAtIndex:
# 自定义UICollectionViewLayout

**总结**
一个UICollectionView的实现包括两个必要部分：UICollectionViewDataSource和UICollectionViewLayout，和一个交互部分：UICollectionViewDelegate。而Apple给出的UICollectionViewFlowLayout已经是一个很强力的layout方案了。

# UICollectionViewLayoutAttributes
property列表：
- @property (nonatomic) CGRect frame
- @property (nonatomic) CGPoint center
- @property (nonatomic) CGSize size
- @property (nonatomic) CATransform3D transform3D
- @property (nonatomic) CGFloat alpha
- @property (nonatomic) NSInteger zIndex
- @property (nonatomic, getter=isHidden) BOOL hidden

UICollectionViewLayoutAttributes的实例中包含了诸如边框，中心点，大小，形状，透明度，层次关系和是否隐藏等信息。当UICollectionView在获取布局时将针对每一个indexPath的部件（包括cell，追加视图和装饰视图），向其上的UICollectionViewLayout实例询问该部件的布局信息。这个布局信息，就以UICollectionViewLayoutAttributes的实例的方式给出。

# UICollectionViewLayout
UICollectionViewLayout的功能为向UICollectionView提供布局信息，不仅包括cell的布局信息，也包括追加视图和装饰视图的布局信息。实现一个自定义layou的常规做法是继承UICollectionViewLayout类，然后重载下列方法。

- - (CGSize)collectioonViewContentSize
	+ 返回collctionView的内容尺寸
- - (NSArray *)layoutAttributesForElementsInRect:(CGRect)rect
	+ 返回rect中的所有的元素的布局属性
	+ 返回的包括UICollectionViewLayoutAttributes的NSArray
	+ UICollctionViewLayoutAttributes可以是cell，追加视图或装饰视图的信息，通过不同的UICollectionViewLayoutAttributes初始化方法可以得到不同类型的UICollctionViewLayoutAttributes
		- ayoutAttributesForCellWithIndexPath:
		- layoutAttributesForSupplementaryViewOfKind:withIndexPath:
		- layoutAttributesForDecorationViewOfKind:withIndexPath:
- - (UICollectionViewLayoutAttributes *)layoutAttributesForItemAtIndexPath:(NSIndexPath *)indexPath
	+ 返回对应indexPath位置的cell布局属性
- - (UICollectionViewLayoutAttributes *)layoutAttributesForDecorationViewOfKind:(NSString *)decorationViewKind atIndexPath:(NSIndexPath *)indexPath
	+ 返回对应indexPath位置的追加视图的布局属性，如果没有追加视图可以不重载
- - (UICollectionViewLayoutAttributes *)layoutAttributesForDecorationViewOfKind:(NSString *)decorationViewKind atIndexPath:(NSIndexPath *)indexPath
	+ 返回对应于indexPath的位置的装饰视图的布局属性，如果没有装饰视图可不重载
- - (BOOL)shouldInvalidateLayoutForBoundsChange:(CGRect)newBounds
	+ 当边界发生改变时，是否应该刷新布局。

在初始化一个UICollectionViewLayout实例后，会有一系列的准备方法被自动调用，以保证layout实例的正确
1. -(void)prepareLayout，默认下该方法是每都不做，但是在自己的子类实现中，一般在该方法中设定一些必要的layout的结构和初始需要的参数等。
2. -(CGSize)collectionViewContentSize，以确定collection应该占据的尺寸，注意这里的尺寸不是指可使部分的尺寸，而是所有内容所占的尺寸。
3. -(NSArray *)layoutAttributesForElementsInRect:(Rect)rect被调用，初始的layout外观将由该方法返回的UICollctionViewLayoutAttributes来决定。

另外，在需要更新layout时，需要给当前layout发送 -invalidateLayout，该消息会立即返回，并且预约在下一个loop的时候刷新当前layout。在-invalidateLayout后的下一个collectionView的刷新loop中，又会从prepareLayout开始，依次再调用-collectionViewContentSize和-layoutAttributesForElementsInRect来生成更新后的布局。

### LineLayout——对于个别UICollectionViewLayoutAttributes的调整

```Objective-C
// LinrLayout.m
# import "LineLayout.h"


# define ITEM_SIZE 200.0
# define ACTIVE_DISTANCE 200
# define ZOOM_FACTOR 0.4

@implementation LineLayout

-(id)init
{
    self = [super init];
    if (self) {
        self.itemSize = CGSizeMake(ITEM_SIZE, ITEM_SIZE);
        self.scrollDirection = UICollectionViewScrollDirectionHorizontal;
        //  确定了缩进，此处为上方、下方各缩进200
        self.sectionInset = UIEdgeInsetsMake(ITEM_SIZE, ITEM_SIZE/2, ITEM_SIZE, ITEM_SIZE/2);
        //  每个item在水平方向的最小间距
        self.minimumLineSpacing = ITEM_SIZE/4;
        
    }
    return self;
}
// 边界改变时是否重新排版
- (BOOL)shouldInvalidateLayoutForBoundsChange:(CGRect)oldBounds
{
    return YES;
}
//  初始的layout外观将由该方法返回的UICollctionViewLayoutAttributes来决定
-(NSArray*)layoutAttributesForElementsInRect:(CGRect)rect
{
    NSArray* array = [super layoutAttributesForElementsInRect:rect];
    CGRect visibleRect;
    visibleRect.origin = self.collectionView.contentOffset;
    visibleRect.size = self.collectionView.bounds.size;
    for (UICollectionViewLayoutAttributes* attributes in array) {
        if (CGRectIntersectsRect(attributes.frame, rect)) {
            CGFloat distance = CGRectGetMidX(visibleRect) - attributes.center.x;
            CGFloat normalizedDistance = distance / ACTIVE_DISTANCE;
            if (ABS(distance) < ACTIVE_DISTANCE) {
                CGFloat zoom = 1 + ZOOM_FACTOR*(1 - ABS(normalizedDistance));
                attributes.transform3D = CATransform3DMakeScale(zoom, zoom, 1.0);
                attributes.zIndex = 1;
            }
        }
    }
    return array;
}

//  自动对齐到网格
- (CGPoint)targetContentOffsetForProposedContentOffset:(CGPoint)proposedContentOffset withScrollingVelocity:(CGPoint)velocity
{
    //  proposedContentOffset是没有对齐到网格时本来应该停下来的位置
    CGFloat offsetAdjustment = MAXFLOAT;
    CGFloat horizontalCenter = proposedContentOffset.x + (CGRectGetWidth(self.collectionView.bounds) / 2.0);
    //  当前显示的区域
    CGRect targetRect = CGRectMake(proposedContentOffset.x, 0.0, self.collectionView.bounds.size.width, self.collectionView.bounds.size.height);
    //  取当前显示的item
    NSArray* array = [super layoutAttributesForElementsInRect:targetRect];
    //  对当前屏幕中的UICollectionViewLayoutAttributes逐个与屏幕中心进行比较，找出最接近中心的一个
    for (UICollectionViewLayoutAttributes* layoutAttributes in array) {
        CGFloat itemHorizontalCenter = layoutAttributes.center.x;
        if (ABS(itemHorizontalCenter - horizontalCenter) < ABS(offsetAdjustment)) {
            offsetAdjustment = itemHorizontalCenter - horizontalCenter;
        }
    }    
    return CGPointMake(proposedContentOffset.x + offsetAdjustment, proposedContentOffset.y);
}

@end
```
[例子](https://github.com/zt1991616/LineLayout)

### CircleLayout——完全自定义的Layout，添加删除item，以及手势识别

```Objective-C
# import "CircleLayout.h"

# define ITEM_SIZE 70

@implementation CircleLayout

// 准备计算需要的参数
-(void)prepareLayout
{
    [super prepareLayout];
    
    CGSize size = self.collectionView.frame.size;
    _cellCount = [[self collectionView] numberOfItemsInSection:0];
    // 中心点
    _center = CGPointMake(size.width / 2.0, size.height / 2.0);
    // 半径
    _radius = MIN(size.width, size.height) / 2.5;
}
// collctionView的内容大小就是collectionView的大小
-(CGSize)collectionViewContentSize
{
    return [self collectionView].frame.size;
}
// 通过所在的indexPath确定位置
- (UICollectionViewLayoutAttributes *)layoutAttributesForItemAtIndexPath:(NSIndexPath *)path
{
    UICollectionViewLayoutAttributes* attributes = [UICollectionViewLayoutAttributes layoutAttributesForCellWithIndexPath:path];
    attributes.size = CGSizeMake(ITEM_SIZE, ITEM_SIZE);
    // 配置attributes到圆周上
    attributes.center = CGPointMake(_center.x + _radius * cosf(2 * path.item * M_PI / _cellCount),
                                    _center.y + _radius * sinf(2 * path.item * M_PI / _cellCount));
    return attributes;
}
// 用来在一开始给出一套UICollectionViewLayoutAttributes
-(NSArray*)layoutAttributesForElementsInRect:(CGRect)rect
{
    NSMutableArray* attributes = [NSMutableArray array];
    for (NSInteger i=0 ; i < self.cellCount; i++) {
        //
        NSIndexPath* indexPath = [NSIndexPath indexPathForItem:i inSection:0];
        [attributes addObject:[self layoutAttributesForItemAtIndexPath:indexPath]];
    }    
    return attributes;
}
// 插入前，cell在圆心位置，全透明
- (UICollectionViewLayoutAttributes *)initialLayoutAttributesForInsertedItemAtIndexPath:(NSIndexPath *)itemIndexPath
{
    UICollectionViewLayoutAttributes* attributes = [self layoutAttributesForItemAtIndexPath:itemIndexPath];
    attributes.alpha = 0.0;
    attributes.center = CGPointMake(_center.x, _center.y);
    return attributes;
}

// 删除时，cell在圆心位置，全透明，且只有原来的1/10大
- (UICollectionViewLayoutAttributes *)finalLayoutAttributesForDeletedItemAtIndexPath:(NSIndexPath *)itemIndexPath
{
    UICollectionViewLayoutAttributes* attributes = [self layoutAttributesForItemAtIndexPath:itemIndexPath];
    attributes.alpha = 0.0;
    attributes.center = CGPointMake(_center.x, _center.y);
    attributes.transform3D = CATransform3DMakeScale(0.1, 0.1, 1.0);
    return attributes;
}

@end
```

```Objective-C
# import "ViewController.h"
# import "Cell.h"

@implementation ViewController

-(void)viewDidLoad
{
    self.cellCount = 20;
    // 添加一个手势识别
    UITapGestureRecognizer* tapRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(handleTapGesture:)];
    [self.collectionView addGestureRecognizer:tapRecognizer];
    [self.collectionView registerClass:[Cell class] forCellWithReuseIdentifier:@"MY_CELL"];
    [self.collectionView reloadData];
    self.collectionView.backgroundColor = [UIColor greenColor];
}

- (NSInteger)collectionView:(UICollectionView *)view numberOfItemsInSection:(NSInteger)section;
{
    return self.cellCount;
}

- (UICollectionViewCell *)collectionView:(UICollectionView *)cv cellForItemAtIndexPath:(NSIndexPath *)indexPath;
{
    Cell *cell = [cv dequeueReusableCellWithReuseIdentifier:@"MY_CELL" forIndexPath:indexPath];
    return cell;
}

- (void)handleTapGesture:(UITapGestureRecognizer *)sender {
    
    if (sender.state == UIGestureRecognizerStateEnded)
    {
        CGPoint initialPinchPoint = [sender locationInView:self.collectionView];
        NSIndexPath* tappedCellPath = [self.collectionView indexPathForItemAtPoint:initialPinchPoint];
        // 点击处没有cell
        if (tappedCellPath!=nil)
        {
            self.cellCount = self.cellCount - 1;
            [self.collectionView performBatchUpdates:^{
                [self.collectionView deleteItemsAtIndexPaths:[NSArray arrayWithObject:tappedCellPath]];
                
            } completion:nil];
        }
        else
        {
            self.cellCount = self.cellCount + 1;
            [self.collectionView performBatchUpdates:^{
                [self.collectionView insertItemsAtIndexPaths:[NSArray arrayWithObject:[NSIndexPath indexPathForItem:0 inSection:0]]];
            } completion:nil];
        }
    }
}

@end
```
[例子](https://github.com/zt1991616/CircleLayout)