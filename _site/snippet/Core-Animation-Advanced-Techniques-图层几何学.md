## Core Animation Advanced Techniques 图层几何学笔记

###　图层布局

#### 基本几何属性

视图的 `frame` `bounds` `center` 属性仅仅是存取方法,当操作视图的 `frame` ,实际上是在
改变位于视图下的 `CALayer` 的 `frame`,不能独立与图层之外改变视图的frame.

对于视图或者图层来说, `frame` 其实是一个虚拟的属性,是根据 `bounds` `position` `transform`
计算出来的.

对图层做变换时,比如旋转或者缩放, `frame` 实际上代表了覆盖在图层旋转之后的整个轴对其的矩形
区域,也就是说 `frame` 的宽高可能和 `bounds` 的宽高不再一致了.

![](http://7u2qbg.com1.z0.glb.clouddn.com/Core-Animation-Advanced-Techniques-几何学_01.png)

#### 锚点
`anchorPoint`  是移动图层的把柄.
