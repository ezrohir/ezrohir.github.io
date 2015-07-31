## Core Animation Advanced Techniques 视觉效果

### 圆角
CALayer `cornerRadius` 控制这图层角的曲率,默认情况下这个曲率值只影响背景颜色而不影响背景
图片或者是子视图.配合 `masksToBounds` 可以把图层你所有的东西截取.

使用图层蒙板 `CAShapeLayer` 也是改变图层圆角的手段之一.

### 图层边框
`borderWidth` `borderColor`

### 阴影
`shadowOpacity` 默认值为0,也就是没阴影,大于0时候图层会显示出阴影效果.
`shadowColor`
`shadowOffset` 控制这阴影的方向和距离,默认是{0,-3},表示相对Y轴有3个点的上位移.
`shadowRadius` 控制这阴影的模糊度.
和图层边框不同,图层的阴影继承自内容的外形,Core Animation 会将寄宿图考虑在内,
搭配图层形状创建一个阴影.
![](http://7u2qbg.com1.z0.glb.clouddn.com/Core-Animation-Advanced-Techniques-视觉效果_01.png)

### shadowPath 属性
图层阴影并不中是方的,而是从图层内容的形状继承而来,但是实时计算阴影
也是一个非常消耗资源的事情.如果实现知道阴影形状,可以通过制定一个 `shadowPath`
来提高性能.

```
- (void)viewDidLoad
{
    [super viewDidLoad];

    //enable layer shadows
    self.layerView1.layer.shadowOpacity = 0.5f;
    self.layerView2.layer.shadowOpacity = 0.5f;

    //create a square shadow
    CGMutablePathRef squarePath = CGPathCreateMutable();
    CGPathAddRect(squarePath, NULL, self.layerView1.bounds);
    self.layerView1.layer.shadowPath = squarePath; CGPathRelease(squarePath);

    ￼//create a circular shadow
    CGMutablePathRef circlePath = CGPathCreateMutable();
    CGPathAddEllipseInRect(circlePath, NULL, self.layerView2.bounds);
    self.layerView2.layer.shadowPath = circlePath; CGPathRelease(circlePath);
}
```
如果是一个矩形或者是圆,用 `CGPath` 会相当简单明了.但是复杂一点的图形,
`UIBezierPath` 会更合适,它是CGPath在UIKit框架的封装.

#### 图层蒙板
CALayer 的 `mask` 属性定义了父图层的部分可见区域.
![](http://7u2qbg.com1.z0.glb.clouddn.com/Core-Animation-Advanced-Techniques-视觉效果_02.png)


#### 透明
UIView 的 `alpha` CALayer 的 `opacity`
