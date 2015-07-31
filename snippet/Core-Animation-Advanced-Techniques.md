## Core Animation Advanced Techniques 寄宿图笔记

### 寄宿图基本
- CALayer的寄宿图是指图层中包含的图.
主要通过 `contents` 属性实现.如:

  ```layer.contents = (__bridge id)image.CGImage;```

- CALayer 与 UIView 的 `contentMode` 对应的属性为 `contentsGravity`
- `contentsScale` 属于支持 retina 屏幕机制的一部分,它用来判断在绘制图层时候需要
  为寄宿图创建的控件大小,图片的拉伸度.但此属性的优先级低于 `contentsGravity`.

  如果 `cotentsScale` 为1.0,则会以每个点1哥小鼠绘制图片,如果2.0.则以每个点2哥像素
  绘制图片.此属性和  `contentsGravity` 没有必然联系,职责不同.

- 对应与 UIView 的 `clipsToBounds` CALayer 提供了 `maskToBounds` 来决定
  是否裁剪超过视图边界的内容.
- `contentsRect` 一个能黑魔法的属性,能显示寄宿图的一个子域.

  ```
  // 取寄宿图左上1/4
  layer.contents = (__bridge id)image.CGImage;
  layer.contentsRect = withContentRect:CGRectMake(0, 0, 0.5, 0.5)
  ```

- `contentsCenter` 效果和 UIImage 的 `resizableImaegWithCapInsets:` 相似,具体需要
  写代码体会了下.

### 提供寄宿图的若干方法
+ 通过 `contents` 属性赋值CGImage
+ 继承UIView并实现 `-drawRect:` 方法,它会为视图分配一个寄宿图,像素尺寸等于视图大小乘以
  `contentsScale` ,所以不要留空的 `drawRect` 方法,会浪费内存.
+ 通过CAlayer 的 delegate 方法提供寄宿图.
  ```
  (void)displayLayer:(CAlayer *)layer;
  (void)drawLayer:(CAlayer *)layer inContext:(CGContextRef)ctx;
  ```
