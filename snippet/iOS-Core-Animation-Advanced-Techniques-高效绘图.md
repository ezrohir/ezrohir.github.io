## Core Animation Advanced Techniques 高效绘图
###　软件绘图
iOS 中,软件绘图一般由 `Core Graphics` 框架绘制,相比 Core Animation 和 OpenGL ,Core Graphics
要慢不少.

软件绘图不仅效率低,还会消耗可观的内存.CALayer只需要一些与自己相关的内存:
只有它的寄宿图会消耗一定的内存空间.即使直接赋给contents属性一张图片,也不需要增加额外的照片存储大小.
如果相同的一张图片被多个图层作为contents属性,那么他们将会共用同一块内存,而不是复制内存块.

但是一旦你实现了CALayerDelegate协议中的-drawLayer:inContext:方法或者UIView中的-drawRect:方法（其实就是前者的包装方法,
图层就创建了一个绘制上下文.

软件绘图的代价昂贵,除非绝对必要,你应该避免重绘你的视图.提高绘制性能的秘诀就在于尽量避免去绘制.

### 异步绘制
`CATiledLayer` `drawsAsynchronously`
