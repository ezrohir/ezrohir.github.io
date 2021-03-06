---
layout: post
title: Duplicate Symbol问题
---
## Duplicate Symbol xxx 原因和解决
今天引入新的静态库时发生Duplicate Symbol xxx，检查了下工程配置，other linker flag-> -all_load.(此处是铺垫)。然后网上参考了些资料，见尾部。

### Duplicate Symbol原因
看报错描述大意知道是俩框架某个Symbol冲突了，那么问题来了，这个冲突的Symbol代表的是什么，函数还是类？其实仔细想想就答案了，Objective-C 这种runtime机制的语言Symbol怎么可能是函数，苹果文档也解释了这点:
>Objective-C does not define linker symbols for each function (or method, in Objective-C) - instead, linker symbols are only generated for each class. 

### Duplicate Symbol解决
OK，现在说说看到的解决方法。

1. 工具解压静态库，删除相同编译文件，重新打包。如果的时间多想玩玩或者老板让这么干(我不是在黑老板)，[参考在这](http://atnan.com/blog/2012/01/12/avoiding-duplicate-symbol-errors-during-linking-by-removing-classes-from-static-libraries)	.
2. 修改other linker flag，常见的other linker flag有`-ObjC `,`-all_load `,`-force_load `.
	+ -ObjC  -ObjC allow the static library to use objective-c specific stuffs like kvc or categories.
	+ -all_load   Loads all members of static archive libraries.
	+ -force_load   -all_load forces all members of all archives to be loaded. This option allows you to target a specific archive.

看解释真没发现-ObjC和-all_load有什么区别，不过事实上other linker flag-> -all_load就会报Duplicate Symbol xxx，而other linker flag-> --ObjC编译通过，只能大胆假设-ObjC下linker会链接所有静态库文件，但不管多少静态库，有没有重复，反正静态库中文件保证只被链接一次，有点GCD中dispatch_once的味道。在求证过程中对-all_load的描述都指向[Technical Q&A QA1490](https://developer.apple.com/library/mac/qa/qa1490/_index.html)，但是该Document已修改，感觉苹果应该修正了这个问题。

PS -all_load引入的原因
>Important: For 64-bit and iPhone OS applications, there is a linker bug that prevents -ObjC from loading objects files from static libraries that contain only categories and no classes. The workaround is to use the -allload or -forceload flags.

Anyway,`-ObjC`,`-force_load `都是快速解决问题的方法。至于`-ObjC `与`-all_load `,`-force_load `的区别，有空再研究下。


### 参考引用

[Technical Q&A QA1490
Building Objective-C static libraries with categories](https://developer.apple.com/library/mac/qa/qa1490/_index.html)

[avoiding-duplicate-symbol-errors-during-linking-by-removing-classes-from-static-libraries](http://atnan.com/blog/2012/01/12/avoiding-duplicate-symbol-errors-during-linking-by-removing-classes-from-static-libraries)

[Objective-C categories in static library](http://stackoverflow.com/questions/2567498/objective-c-categories-in-static-library)
