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
