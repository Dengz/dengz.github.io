---
style: post
title: 在IOS的UIWEBVIEW中调试JAVASCRIPT
---

一般而言，调试javascript这活应该是在桌面的浏览器上进行的，上面有完备的工具，就像firefox的firebug插件，chrome的“审查元素”，就连opera也提供了类似的后台工具。

但是，我们在桌面上调试好了JS代码，用iOS的UIWebView加载后出错怎么办？UIWebViewDelegate提供的几个方法：

```objective-c
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType;

- (void)webViewDidStartLoad:(UIWebView *)webView;

- (void)webViewDidFinishLoad:(UIWebView *)webView;

- (void)webView:(UIWebView *)webView didFailLoadWithError:(NSError *)error;
```

似乎都不处理js出错的情况。在stackoverflow上找到了如下的[解决方法](http://stackoverflow.com/questions/193119/how-can-my-iphone-objective-c-code-get-notified-of-javascript-errors-in-a-uiwebv/193282#193282)， 记录一下：

1. 首先创建一个UIWebView的子类
2. 重载方法：

```objective-c
- (void)webView:(id)webView windowScriptObjectAvailable:(id)newWindowScriptObject {
    // save these goodies
    windowScriptObject = newWindowScriptObject;
    privateWebView = webView;

    if (scriptDebuggingEnabled) {
        [webView setScriptDebugDelegate:[[YourScriptDebugDelegate alloc] init]];
    }
}
```

3. 定义并实现YourScriptDebugDelegate

```objective-c
- (void)webView:(WebView *)webView       didParseSource:(NSString *)source
 baseLineNumber:(unsigned)lineNumber
        fromURL:(NSURL *)url
       sourceId:(int)sid
    forWebFrame:(WebFrame *)webFrame
{
    NSLog(@"NSDD: called didParseSource: sid=%d, url=%@", sid, url);
}

// some source failed to parse
- (void)webView:(WebView *)webView  failedToParseSource:(NSString *)source
 baseLineNumber:(unsigned)lineNumber
        fromURL:(NSURL *)url
      withError:(NSError *)error
    forWebFrame:(WebFrame *)webFrame
{
    NSLog(@"NSDD: called failedToParseSource: url=%@ line=%d error=%@\nsource=%@", url, lineNumber, error, source);
}

- (void)webView:(WebView *)webView   exceptionWasRaised:(WebScriptCallFrame *)frame
       sourceId:(int)sid
           line:(int)lineno
    forWebFrame:(WebFrame *)webFrame
{
    NSLog(@"NSDD: exception: sid=%d line=%d function=%@, caller=%@, exception=%@", 
          sid, lineno, [frame functionName], [frame caller], [frame exception]);
}
```

以上就能通过ScriptDebugDelegate来定位js的错误了
