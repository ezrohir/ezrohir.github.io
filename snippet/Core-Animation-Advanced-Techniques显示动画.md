## Core Animation Advanced Techniques 显示动画
### 属性动画
属性动画作用与图层的某个单一属性,并指定了它的一个目标值,或者一连串将要动画的值.
属性动画分为两种:基础和关键帧.

#### 基础动画
`CAAnimation` `CAPropertyAnimation` `CABasicAnimation`
- `CAAnimation` 抽象类,提供了计时函数,反馈动画状态的委托, `removeOnCompletion`.
- `CAPropertyAnimation` 通过 `keyPath` 指定动画属性.
- `CABasicAnimation` 继承与 `CAPropertyAnimation`,添加了如下属性
  ```
  id fromValue
  id toValue
  id byValue
  ```
#### 关键帧动画
`CAkeyframeAnimation` 同样是 `CAPropertyAnimation` 的一个之类,它可以根据一连串随意的值
来做动画.
```
- (IBAction)changeColor
{
    //create a keyframe animation
    CAKeyframeAnimation *animation = [CAKeyframeAnimation animation];
    animation.keyPath = @"backgroundColor";
    animation.duration = 2.0;
    animation.values = @[
                         (__bridge id)[UIColor blueColor].CGColor,
                         (__bridge id)[UIColor redColor].CGColor,
                         (__bridge id)[UIColor greenColor].CGColor,
                         (__bridge id)[UIColor blueColor].CGColor ];
    //apply animation to layer
    [self.colorLayer addAnimation:animation forKey:nil];
}
```
除了数组来描述动画, `CAKeyframeAnimation` 还提供了 `path` 属性来直观的描述帧动画.
```
- (void)viewDidLoad
{
    [super viewDidLoad];
    //create a path
    UIBezierPath *bezierPath = [[UIBezierPath alloc] init];
    [bezierPath moveToPoint:CGPointMake(0, 150)];
    [bezierPath addCurveToPoint:CGPointMake(300, 150) controlPoint1:CGPointMake(75, 0) controlPoint2:CGPointMake(225, 300)];
    //draw the path using a CAShapeLayer
    CAShapeLayer *pathLayer = [CAShapeLayer layer];
    pathLayer.path = bezierPath.CGPath;
    pathLayer.fillColor = [UIColor clearColor].CGColor;
    pathLayer.strokeColor = [UIColor redColor].CGColor;
    pathLayer.lineWidth = 3.0f;
    [self.containerView.layer addSublayer:pathLayer];
    //add the ship
    CALayer *shipLayer = [CALayer layer];
    shipLayer.frame = CGRectMake(0, 0, 64, 64);
    shipLayer.position = CGPointMake(0, 150);
    shipLayer.contents = (__bridge id)[UIImage imageNamed: @"Ship.png"].CGImage;
    [self.containerView.layer addSublayer:shipLayer];
    //create the keyframe animation
    CAKeyframeAnimation *animation = [CAKeyframeAnimation animation];
    animation.keyPath = @"position";
    animation.duration = 4.0;
    animation.path = bezierPath.CGPath;
    [shipLayer addAnimation:animation forKey:nil];
}
```
上述示例中,图片运动时方向永远是向右的,而不是沿着曲线切线方向,对此 `CAKeyFrameAnimation` 提供
了 `rotationMode` 属性, `kCAAnimationRotateAuto` 会使得图片自动沿切线方向旋转.

#### 动画组
使用 `CAAnimationGroup` 来组合动画.


#### 过渡
如果要改变一个不能动画的属性,或者从层级关系中添加或者移除图层,属性动画是办不到的.
作为补充, iOS 提供了过渡的概念,过渡并不像属性那样平滑的在两个值之间做动画,而是影响到
整个图层的变化.
创见过渡动画,需使用 `CATransition` ,是 `CAAnimation` 的子类,两重要的属性 `type` `subtype`
```
// type
kCATransitionFade
kCATransitionMoveIn
kCATransitionPush
kCATransitionReveal


// subtype
kCATransitionFromRight
kCATransitionFromLeft
kCATransitionFromTop
kCATransitionFromBottom


// examples
self.images = ....
//set up crossfade transition
CATransition *transition = [CATransition animation];
transition.type = kCATransitionFade;
//apply transition to imageview backing layer
[self.imageView.layer addAnimation:transition forKey:nil];
//cycle to next image
UIImage *currentImage = self.imageView.image;
NSUInteger index = [self.images indexOfObject:currentImage];
index = (index + 1) % [self.images count];
self.imageView.image = self.images[index];
```

#### 对图层树的动画
`CATransition` 并不作用于指定的图层属性，这就是说你可以在即使不能准确得知改变了什么的情况下对图层做动画.

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    self.window = [[UIWindow alloc] initWithFrame: [[UIScreen mainScreen] bounds]];
    UIViewController *viewController1 = [[FirstViewController alloc] init];
    UIViewController *viewController2 = [[SecondViewController alloc] init];
    self.tabBarController = [[UITabBarController alloc] init];
    self.tabBarController.viewControllers = @[viewController1, viewController2];
    self.tabBarController.delegate = self;
    self.window.rootViewController = self.tabBarController;
    [self.window makeKeyAndVisible];
    return YES;
}
- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController
{
    ￼//set up crossfade transition
    CATransition *transition = [CATransition animation];
    transition.type = kCATransitionFade;
    //apply transition to tab bar controller's view
    [self.tabBarController.view.layer addAnimation:transition forKey:nil];
}
```

#### 在动画过程中取消动画
```
- (CAAnimation *)animationForKey:(NSString *)key;
- (void)removeAnimationForKey:(NSString *)key;
- (void)removeAllAnimations;
```
动画一旦被移除,图层的外观就立刻更新到当前的模型图层的值.
动画在结束之后默认会被移除,除非设置 `removeOnComplete` 为 NO.
