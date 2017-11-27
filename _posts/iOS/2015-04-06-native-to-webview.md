---
layout: post
title: iOS Native 与 JavaScript 交互
date:   2015-04-06 22:14:54
comments: true
categories: iOS
---


#  iOS Native 与 JavaScript 交互

iOS提供的 webView 控件`UIWebView``WKWebView`都提供了与JavaScript交互的能力.<br />
抛开UI展示,iOS7 还提供了 JavaScriptCore 库,封装了WebKit的JavaScript引擎,赋予了在iOS App中调用 JavaScript代码的能力.<br />

## UIWebView
我们都对`UIWebView`API很熟悉.[WebViewJavascriptBridge](https://github.com/marcuswestin/WebViewJavascriptBridge)是老牌的桥接库了,最近还在更新.<br />
具体实现WebViewJavascriptBridge 反复利用 UIWebView 的`stringByEvaluatingJavaScriptFromString`方法.对于 Native 调用 JavaScript ,大概思路是:

1. WebViewJavascriptBridge injectJavascriptFile ,在 JavaScript 中初始化保存注册方法名字的 messageHandlers 对象.<br />
2. 当如下调用 JavaScript 代码时<br />
   ```
   [_bridge callHandler:@"testJavascriptHandler" data:data responseCallback:^(id response) {
        NSLog(@"testJavascriptHandler responded: %@", response);
    }];
   ```
    <br />
   上述代码会最终转换为<br />
   ```
   [_webView stringByEvaluatingJavaScriptFromString:javascriptCommand];
   ```
   <br />
   而 javascriptCommand 一般是某种形式JavaScript调用,某个案例是
    ```
     WebViewJavascriptBridge._handleMessageFromObjC('{\"callbackId\":\"objc_cb_1\",\"data\":{\"greetingFromObjC\":\"Hi there, JS!\"},\"handlerName\":\"testJavascriptHandler\"}');
    ```
    <br />
3. 上述会引发执行 WebViewJavascriptBridge_JS.m 中的 `_handleMessageFromObjC` JavaScript 方法,大概是映射到 JavaScript 注册的 testJavascriptHandler 方法,执行   testJavascriptHandler 方法,结果存储.<br />
4. 3中的方法执行完毕,会伪造一个自定义的 url 跳转.被 `UIWebView` 拦截,解析是否是自定义的 url ,是的话 执行命令`stringByEvaluatingJavaScriptFromString` 参数是`WebViewJavascriptBridge._fetchQueue()`.<br />
5. 4步骤执行的结果是取3步骤的存储结果,回调 responseCallback ,一次完成的调用Native 调用 H5 过程结束.<br />

具体可以断点查看源码. JavaScript 调用 Native 方法大同小异.

## WKWebView
相比之下, WKWebView API要友好的多.<br />
WKWebView 提供了 WKScriptMessageHandler ,JavaScript能通过改对象Post信息到 WKWebView 实例.<br />
而 WKWebView 调用 JavaScript ,还是通过 `- evaluateJavaScript:completionHandler:` 方法.<br />
注意该方法提供了一个 block 回调,这有别与 UIWebView 的 `- stringByEvaluatingJavaScriptFromString:` 方法,其返回 NSString .<br />
那么从 API 主观上看起来 WKWebView 调用 `- evaluateJavaScript:completionHandler:`  是异步的,而 UIWebView 调用 `- stringByEvaluatingJavaScriptFromString:` 是同步的. <br />

WKWebView 的配置比较流程化

{% highlight lua %}
// 基本配置
WKWebViewConfiguration *configuration = [[WKWebViewConfiguration alloc]
                                         init];
WKUserContentController *controller = [[WKUserContentController alloc]
                                       init];

[controller addScriptMessageHandler:self name:@"observe"];
configuration.userContentController = controller;

NSURL *jsbin = [NSURL URLWithString:E_WEB_URL];

_webView = [[WKWebView alloc] initWithFrame:self.view.frame
                              configuration:configuration];

[_webView loadRequest:[NSURLRequest requestWithURL:jsbin]];
[self.view addSubview:_webView];

// JavaScript 回调
- (void)userContentController:(WKUserContentController *)userContentController
      didReceiveScriptMessage:(WKScriptMessage *)message {
    if ([message.name isEqualToString:@"observe"]) {
        NSLog(@"Received event %@", message.body);

    }
}
{% endhighlight %}

在JavaScript中如下调用Post信息到客户端

{% highlight lua %}
window.webkit.messageHandlers.observe.postMessage(message_object_send_to_client);
{% endhighlight %}

单从 API 设计上看,WKWebView 比 UIWebView 不知高到哪里去了.

## JavaScriptCore
Nshipster 上有很好的[文章](http://nshipster.cn/javascriptcore/).


## 其他
以上只是 iOS Native 与 JavaScript 交互的通信渠道,在此基层上应封装自己的通信组件.<br />
[CTMediator](https://github.com/casatwy/CTMediator)是简单明了的参考.<br />
安全是个大问题,关注的朋友可以参考[JSPatch](https://github.com/bang590/JSPatch).<br />
本文 Demo 地址 [https://github.com/ezrohir/Native.JavaScript](https://github.com/ezrohir/Native.JavaScript)
也不懂 Dom 的生命周期,所以 Demo 仅供参考.


## 参考
[Using JavaScript with WKWebView in iOS 8](http://www.joshuakehn.com/2014/10/29/using-javascript-with-wkwebview-in-ios-8.html)
