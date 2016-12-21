---
layout: post
title: IOS APP中的COOKIE操作
---

最近有个项目, 需要在app的接口中使用cookie, 这样客户端的接口就和web端的登录态处理就一致了.

NSURLRequest设置cookie和接收cookie这件事情本身很容易, 网上有很多教程了, 比如stackoverflow上的[这篇文章](http://stackoverflow.com/questions/691761/create-a-cookie-for-nsurlrequest), 使用了如下方法:

```objective-c
NSDictionary *properties = [NSDictionary dictionaryWithObjectsAndKeys: 
@"domain.com", NSHTTPCookieDomain,
@"/", NSHTTPCookiePath, // IMPORTANT!
@"testCookies", NSHTTPCookieName,
@"1", NSHTTPCookieValue,
nil];
NSHTTPCookie *cookie = [NSHTTPCookie cookieWithProperties:properties];
NSArray* cookies = [NSArray arrayWithObjects: cookie, nil];
NSDictionary * headers = [NSHTTPCookie requestHeaderFieldsWithCookies:cookies];
[request setAllHTTPHeaderFields:headers];
```

还有这种方法:

```objective-c
NSMutableURLRequest* ret = [NSMutableURLRequest requestWithURL:myURL];
[ret setValue:@"myCookie=foobar" forHTTPHeaderField:@"Cookie"];
```

实际使用的时候不知为何, 服务端都不能正确的接收到cookie字段.

无奈之下只好做了一下盲目的操作…结果…居然…让我试出来了…

"正确"的做法应该是

```objective-c
NSMutableURLRequest* ret = [NSMutableURLRequest requestWithURL:myURL]; 
[ret setValue:@"myCookie=foobar" forHTTPHeaderField:@"cookie"];
```

看出来了吗? key值…

问题也许出在服务器上, 嗯.
