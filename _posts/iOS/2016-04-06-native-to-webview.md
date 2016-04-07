---
layout: post
title: iOS Native 与 JavaScript 交互
date:   2016-04-06 22:14:54
comments: true

---


##  iOS Native 与 JavaScript 交互

iOS提供的 webView 控件`UIWebView``WKWebView`都提供了与JavaScript交互的能力.<br />
抛开UI展示,iOS7 还提供了 JavaScriptCore 库,封装了WebKit的JavaScript引擎,赋予了在iOS App中调用 JavaScript代码的能力.<br />

#### UIWebView
我们都对`UIWebView`API很熟悉.[WebViewJavascriptBridge](https://github.com/marcuswestin/WebViewJavascriptBridge)是老牌的桥接库了,最近还在更新.<br />
具体实现WebViewJavascriptBridge 反复利用 UIWebView 的`stringByEvaluatingJavaScriptFromString`方法.对于 Native 调用 JavaScript ,大概思路是:

1. WebViewJavascriptBridge injectJavascriptFile ,在 JavaScript 中初始化保存注册方法名字的 messageHandlers 对象.<br />
2. 当如下调用 JavaScript 代码时
   ```
   [_bridge callHandler:@"testJavascriptHandler" data:data responseCallback:^(id response) {
        NSLog(@"testJavascriptHandler responded: %@", response);
    }];
   ```

   上述代码会最终转换为
   ```
   [_webView stringByEvaluatingJavaScriptFromString:javascriptCommand];
   ```

   而 javascriptCommand 一般是某种形式JavaScript调用,某个案例是
    ```
     WebViewJavascriptBridge._handleMessageFromObjC('{\"callbackId\":\"objc_cb_1\",\"data\":{\"greetingFromObjC\":\"Hi there, JS!\"},\"handlerName\":\"testJavascriptHandler\"}');
    ```
3. 上述会引发执行 WebViewJavascriptBridge_JS.m 中的 `_handleMessageFromObjC` JavaScript 方法,大概是映射到 JavaScript 注册的 testJavascriptHandler 方法,执行   testJavascriptHandler 方法,结果存储.<br />
4. 3中的方法执行完毕,会伪造一个自定义的 url 跳转.被 `UIWebView` 拦截,解析是否是自定义的 url ,是的话 执行命令`stringByEvaluatingJavaScriptFromString` 参数是`WebViewJavascriptBridge._fetchQueue()`.
5. 4步骤执行的结果是取3步骤的存储结果,回调 responseCallback ,一次完成的调用Native 调用 H5 过程结束.<br />

具体可以断点查看源码. JavaScript 调用 Native 方法大同小异.

#### WKWebView
相比之下,`WKWebView`API要友好的多.<br />
待续

### JavaScriptCore
待续
