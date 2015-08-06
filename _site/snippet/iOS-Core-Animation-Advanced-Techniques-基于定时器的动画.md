# Core Animation Advanced Techniques 基于定时器的动画
`CAdisplayLink` 是 CoreAnimation 提供的您一个类似 `NSTimer` 的类, 它中是在屏幕完成一次更新
之前启动,它实际上就是 `NSTimer` 的内置实现.

用 `CADisplayLink` 而不是 NSTimer 会保证帧率足够连续.
