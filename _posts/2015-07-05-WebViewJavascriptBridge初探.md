---
layout: post
title: WebViewJavascriptBridge 初探
---

一般而言,对 native 调用h5中的js方法, `UIWebView` 有提供API `stringByEvaluatingJavaScriptFromString`
,改方法返回 string 类型的字符串,传入参数为一个 string 类型的js方法.

对于h5中的js与 native 通信,是通过拦截 `UIWebView` 的 `webView:shouldStartLoadWithRequest:navigationType` 方法.

以上为大致的原理, WebViewJavascriptBridge 在此基础上做了写封装.

### 本地调js

WebViewJavascriptBridge 中调用 js 方法,还是通过 `stringByEvaluatingJavaScriptFromString` .如下代码
```
- (void)_sendData:(id)data responseCallback:(WVJBResponseCallback)responseCallback handlerName:(NSString*)handlerName {
    NSMutableDictionary* message = [NSMutableDictionary dictionary];
    if (data) {
        message[@"data"] = data;
    }

    if (responseCallback) {
        NSString* callbackId = [NSString stringWithFormat:@"objc_cb_%ld", ++_uniqueId];
        _responseCallbacks[callbackId] = [responseCallback copy];
        message[@"callbackId"] = callbackId;
    }

    if (handlerName) {
        message[@"handlerName"] = handlerName;
    }
    [self _queueMessage:message];
}
```
1. 将传到js的数据封装下.
2. 将responseCallback 这个block存到本地的一个 `NSMutableDictionary` 容器中,key值为一个每次调用
  `_sendData:responseCallback:handlerName` 自增的int型的标识和特定字符串的组合(`[NSString stringWithFormat:@"objc_cb_%ld", ++_uniqueId]`).
3. 移交给其他函数处理这个message,核心就是调用 `stringByEvaluatingJavaScriptFromString`,期间会有
  数据的转换(json).

### js调本地
上述代码中,responseCallback 的实现是js会伪造一个新url请求,让 `UIWebView` 来拦截.
responseCallback 是js掉本地的方法的一种情况.
```
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {
    if (webView != _webView) { return YES; }
    NSURL *url = [request URL];
    __strong WVJB_WEBVIEW_DELEGATE_TYPE* strongDelegate = _webViewDelegate;
    // 拦截是不是 js 伪造的请求
    if ([[url scheme] isEqualToString:kCustomProtocolScheme]) {
        if ([[url host] isEqualToString:kQueueHasMessage]) {
            [self _flushMessageQueue];
        } else {
            NSLog(@"WebViewJavascriptBridge: WARNING: Received unknown WebViewJavascriptBridge command %@://%@", kCustomProtocolScheme, [url path]);
        }
        return NO;
    } else if (strongDelegate && [strongDelegate respondsToSelector:@selector(webView:shouldStartLoadWithRequest:navigationType:)]) {
        return [strongDelegate webView:webView shouldStartLoadWithRequest:request navigationType:navigationType];
    } else {
        return YES;
    }
}
```
如上述注释,如果有拦截到会调用 `[self _flushMessageQueue]` 方法.
改方法主要是 主动再次掉 `stringByEvaluatingJavaScriptFromString`,js 方法明是 `WebViewJavascriptBridge._fetchQueue`,该方法在js容器中存储了回调数据.其他顺利成章的处理数据流程.如下:
```
- (void)_flushMessageQueue {
    NSString *messageQueueString = [_webView stringByEvaluatingJavaScriptFromString:@"WebViewJavascriptBridge._fetchQueue();"];

    id messages = [self _deserializeMessageJSON:messageQueueString];
    if (![messages isKindOfClass:[NSArray class]]) {
        NSLog(@"WebViewJavascriptBridge: WARNING: Invalid %@ received: %@", [messages class], messages);
        return;
    }
    for (WVJBMessage* message in messages) {
        if (![message isKindOfClass:[WVJBMessage class]]) {
            NSLog(@"WebViewJavascriptBridge: WARNING: Invalid %@ received: %@", [message class], message);
            continue;
        }
        [self _log:@"RCVD" json:message];

        NSString* responseId = message[@"responseId"];
        if (responseId) {
            WVJBResponseCallback responseCallback = _responseCallbacks[responseId];
            responseCallback(message[@"responseData"]);
            [_responseCallbacks removeObjectForKey:responseId];
        } else {
            WVJBResponseCallback responseCallback = NULL;
            NSString* callbackId = message[@"callbackId"];
            if (callbackId) {
                responseCallback = ^(id responseData) {
                    if (responseData == nil) {
                        responseData = [NSNull null];
                    }

                    WVJBMessage* msg = @{ @"responseId":callbackId, @"responseData":responseData };
                    [self _queueMessage:msg];
                };
            } else {
                responseCallback = ^(id ignoreResponseData) {
                    // Do nothing
                };
            }

            WVJBHandler handler;
            if (message[@"handlerName"]) {
                handler = _messageHandlers[message[@"handlerName"]];
            } else {
                handler = _messageHandler;
            }

            if (!handler) {
                [NSException raise:@"WVJBNoHandlerException" format:@"No handler for message from JS: %@", message];
            }

            handler(message[@"data"], responseCallback);
        }
    }
}
```


不懂 js 是硬伤,以上很多都是基于假设,不过假设应该是对的.  
