## Core Animation Advanced Techniques 变换笔记

### 2D变换
`UIView` 的 `transform` 属性是一个 `CGAffineTransform` 类型,用于在二维控件做旋转,缩放,平移.`CGAffineTransform` 类型属于Core Graphics框架.

![](http://7u2qbg.com1.z0.glb.clouddn.com/Core-Animation-Advanced-Techniques-变换_01.png)

对图层应用变换矩阵,图层矩阵内的每一个点都会被相应的变换,形成一个新的四边形.

```
/*
CGAffineTransformMakeRotation(CGFloat angle)
CGAffineTransformMakeScale(CGFloat sx, CGFloat sy)
CGAffineTransformMakeTranslation(CGFloat tx, CGFloat ty)
CGAffineTransformConcat(CGAffineTransform t1, CGAffineTransform t2);
*/

- (void)viewDidLoad
{
    [super viewDidLoad]; //create a new transform
    CGAffineTransform transform = CGAffineTransformIdentity; //scale by 50%
    transform = CGAffineTransformScale(transform, 0.5, 0.5); //rotate by 30 degrees
    transform = CGAffineTransformRotate(transform, M_PI / 180.0 * 30.0); //translate by 200 points
    transform = CGAffineTransformTranslate(transform, 200, 0);
    //apply transform to layer
    self.layerView.layer.affineTransform = transform;
}
```
需要注意的是,变换的结构会影响到之后的变换,所以上面的真实情况是,200像素的向右平移同样也被旋转了30度，缩小了50%，所以它实际上是斜向移动了100像素.

###　3D变换
和 `CGAffineTransform` 类似, `CATransform3D` 是一个矩阵,用于3D变换.

待补充.
