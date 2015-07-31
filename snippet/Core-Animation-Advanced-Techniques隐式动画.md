## Core Animation Advanced Techniques 隐式动画
###　隐式动画实现原理
隐式动画实现的过程,当 `CALayer` 的属性被修改时,它会调用 `-actionForKey:` 方法,传递属性的
名字.
- 图层首先检测它是否有委托,并且是否实现 `CALayerDelegate` 协议中的 `-actionForKey:forKey`
  方法.
- 如果上一步没成立,图层会检测包含属性名称对应行为映射的 `actions` 字典.
- 如果上一步没成立,图层会在 `style` 字典接受搜索属性名.
- 如果上一步没成立,图层将会直接调用定义了每个属性的标准行为方法 `defaultActionForKey:`.

搜索结束后, `-actionForKey:` 要么返回空,要么是 `CAAction` 协议对应的对象,这个对象会被用来做动画.

### 自定义隐私动画
```
self.colorLayer = [CALayer layer];
self.colorLayer.frame = CGRectMake(50.0f, 50.0f, 100.0f, 100.0f);
self.colorLayer.backgroundColor = [UIColor blueColor].CGColor;
//add a custom action
CATransition *transition = [CATransition animation];
transition.type = kCATransitionPush;
transition.subtype = kCATransitionFromLeft;
self.colorLayer.actions = @{@"backgroundColor": transition};
//add it to our view
[self.layerView.layer addSublayer:self.colorLayer];
```

### 呈现与模型
设置 `CALayer` 的属性,实际上是在定义当前事务结束之后图层如何显示的模型.Core Animation
扮演了一个控制器的角色,负责根据图层行为和事务设置去不断更新视图的这些属性在屏幕上的状态.

***图层树*** ***呈现树***
![](http://7u2qbg.com1.z0.glb.clouddn.com/Core-Animation-Advanced-Techniques隐式动画_01.png)
