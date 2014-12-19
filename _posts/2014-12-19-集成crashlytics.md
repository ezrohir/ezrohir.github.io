---
layout: post
title: 集成crashlytics
---
# crash统计工具Crashlytics集成
相比友盟的crash统计，感觉Crashlytics专业得多，但集成过程中，坑还是蛮多的。

### 真机链接Xcode调试Crashlytics是收集不到crash日志的
真机链接Xcode调试中，crash处理会被Xcode的debugger工具接管，虽然在Editor Scheme中可以设置禁用debugger，但最好别这么干，Crashlytics角色是crash收集工具（虽然官网说可以扔掉Xcode debugger了），一般调试还是Xcode的debugger更方便。但对于某些不稳定bug，Crashlytics好像更方便点。

### debug-mode
Crashlytics 的SDK提供`debugMode`属性，初次集成的时候打开debugMode属性终端会输出reports上传到服务器的过程，很方便。

	[Crashlytics sharedInstance].debugMode = YES;

### 友盟也会拦截掉Crashlytics
App crash后，抢hook的真不少。一般App都会用到友盟统计，友盟统计默认开启crash统计，会造成Crashlytics拿不到crash信息，解决如下:
	
	[MobClick setCrashReportEnabled:NO];
	[MobClick startWithAppkey:YOURKEY reportPolicy:YOURPOLICY channelId:YOURCHANNELID];


最后强烈建议集成到slack中，真的很方便。
PS.千万别用平时用的邮箱注册Crashlytics，如果App不是很稳定的话。