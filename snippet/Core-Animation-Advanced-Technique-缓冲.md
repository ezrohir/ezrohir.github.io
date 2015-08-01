## Core Animation Advanced Techniques 缓冲
现实生活中的任何一个物体都会在运动中加速或者减速.那么我们如何在动画中实现这种加速度呢?
一种方法是使用物理引擎来对运动物体的摩擦和动量来建模,然而这会使得计算过于复杂.
我们称这种类型的方程为缓冲函数,幸运的是Core Animation内嵌了一系列标准函数提供给我们使用.

#### CAMediaTimingFunction
通过 `CAAnimation` 的 `timingFunction` 属性,可以使用缓冲方程式. `timingFunction` 是
`CAMediaTimingFunction` 类的对象.
对于隐式动画,可以使用 `CATransaction` 的 `+setAnimationTimingFunction:` 方法.

```
kCAMediaTimingFunctionLinear
kCAMediaTimingFunctionEaseIn
kCAMediaTimingFunctionEaseOut
kCAMediaTimingFunctionEaseInEaseOut
kCAMediaTimingFunctionDefault
```

通过 `+functionWithControlPoints::::` 自定义缓冲动画.
