## Core Animation Advanced Techniques 图层时间
### CAMediaTiming 协议
`CAMediaTiming` 协议定义了在一段时间内用来控制逝去时间的属性的集合, `CALayer` 和 `CAAnimation`
都实现了这个协议,所以时间可以被任意基于一个图层或者一段动画的类控制.
`autoreverses` `repeatCount` `repeatDuration`

### 相对时间
Core Animation里,时间都是相对的,每个动画都有它自己的描述时间,可以独立加速,延迟或者便宜.
`speed` `timeOffset` `beginTime`

### fillMode
`removeOnCompletion` 被设置为NO的动画将会在动画结束的时候仍然保持之前的状态,这就产生一个问题,
当动画开始之前和结束之后,被设置动画的属性将会是什么值?控制这情况的是 `fillMode` 属性.
