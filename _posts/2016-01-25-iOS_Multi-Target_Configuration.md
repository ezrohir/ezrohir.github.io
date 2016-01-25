---
layout: post
title: iOS多Target配置
---

开发过程中我们都有根据不同需求场景构建不同配置App的需求.以我们现在开发的App为例,因为没有使用 TestFight,每天的daylibulid放在fir.im上并通过fir.im提供的构建工具打包上传, 所以有如下环境版本:

- **Beta版** : 提供一些开发调试选项,比如切换 host ,设置数据是否加密
- **AdHoc版本** : 提供 bugTags 之类的问题提交工具
- **AppStore版本** : 纯净版

很显然,不同的 Tagget 的配置是不一样的.手动改的话经常遗漏,比如我在提交1.1版本的时候就忘记移除开发调试选项的bundle发现后不得不重新提交ipa文件,痛定思痛,参考了一些优秀博客,使App支持多了Target的自动配置,以下为一点感悟.

### Preprocessor Macros
在Project->Targets->Build Settings->Apple LLVM7.0 - Preprocessing 中可以设置宏,代码中出镜率很高的 `DEBUG`
```
#ifdef DEBUG
#endif
```
在这里可以发现它的身影.

![](http://7u2qbg.com1.z0.glb.clouddn.com/Screen_Shot_2016-01-25_at_16_06_41.jpg)

同理,可以添加自定义的预处理宏 `ADHOC_DEBUG_TARGET=1`
在配置文件根据环境做不同的设置,比如:
```
#ifdef ADHOC_DEBUG_TARGET
    static NSString *const kAPIURL = ...
#elif ADHOC_RELEASE_TARGET
    static NSString *const kAPIURL = ...
#elif APPSTORE_DEBUG_TARGET
    static NSString *const kAPIURL = ...
#else
    ....
#endif
```

### User-Defined Settings
在集成小米推送服务中发现了 `User-Defined` 的使用,依葫芦话瓢,`Preprocessor Macros` 实现的功能也可以换种方法实现.

![](http://7u2qbg.com1.z0.glb.clouddn.com/Screen_Shot_2016-01-25_at_16_23_12.jpg)

那么怎么取得 `DEFINEDENV` 对应的值.如下代码:
```
NSString* env = [[[NSBundle mainBundle] infoDictionary] valueForKey:@"DEFINEDENV"];
NSLog(@"env is %@",env);
```
取得 `DEFINEDENV` 值以后,然后根据实际需要做自定义操作.

相比较, `Preprocessor Macros` 和 `User-Defined Settings` 使用场景还是有区别的.本人实际也没什么经验.

主观感受　`Preprocessor Macros` 用的多的地方可能是自定义配置.

对于 `User-Defined Settings` ,我们有时候需要根据测试环境和生成环境不同设置不同图片,或者不同的　`Target` 使用不同的图片.这时候它很适用:
```
(NSString *)getEnvImageName:(NSString *)imageName
{
    NSString* suffix = [[[NSBundle mainBundle] infoDictionary] valueForKey:@"IMAGE_SUFFIX"];
    return [imageName stringByAppendingString:suffix];
}
```

### 其他
以上仅是实际使用的一些皮毛,对于 Xcode 中的 `Target` 的 configure 设置怎么链入编译器,是个很深的话题,回头再研究.
