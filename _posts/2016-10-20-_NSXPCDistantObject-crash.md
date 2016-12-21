---
layout: post
title: Crash about [_NSXPCDistantObject methodSignatureForSelector:]
---

Last few months this crash came across to my app, the crash stack show as below.

```objective-c
Fatal Exception: NSInvalidArgumentException
*** -[_NSXPCDistantObject methodSignatureForSelector:]: No protocol has been set on connection <NSXPCConnection: 0x151e1d600> connection to service named com.apple.nsurlsessiond

0  CoreFoundation                 0x183971900 __exceptionPreprocess
1  libobjc.A.dylib                0x182fdff80 objc_exception_throw
2  CoreFoundation                 0x183971848 -[NSException initWithCoder:]
3  Foundation                     0x184244e7c -[_NSXPCDistantObject methodSignatureForSelector:]
4  CoreFoundation                 0x183975324 ___forwarding___
5  CoreFoundation                 0x18387968c _CF_forwarding_prep_0
6  CoreFoundation                 0x183916fc4 __CFNOTIFICATIONCENTER_IS_CALLING_OUT_TO_AN_OBSERVER__
7  CoreFoundation                 0x1839167e4 _CFXRegistrationPost
8  CoreFoundation                 0x183916564 ___CFXNotificationPost_block_invoke
9  CoreFoundation                 0x18397bde4 -[_CFXNotificationRegistrar find:object:observer:enumerator:]
10 CoreFoundation                 0x1838570f4 _CFXNotificationPost
11 Foundation                     0x184246d2c -[NSNotificationCenter postNotificationName:object:userInfo:]
12 UIKit                          0x1888e79f8 -[UIApplication _sendWillEnterForegroundCallbacks]
13 UIKit                          0x188921988 -[UIApplication _handleApplicationActivationWithScene:transitionContext:completion:]
14 UIKit                          0x188921038 -[UIApplication _handleApplicationLifecycleEventWithScene:transitionContext:completion:]
15 UIKit                          0x18890b588 __70-[UIApplication scene:didUpdateWithDiff:transitionContext:completion:]_block_invoke
16 UIKit                          0x18890b210 -[UIApplication scene:didUpdateWithDiff:transitionContext:completion:]
17 FrontBoardServices             0x184f27790 -[FBSSerialQueue _performNext]
18 FrontBoardServices             0x184f27b10 -[FBSSerialQueue _performNextFromRunLoopSource]
19 CoreFoundation                 0x183928efc __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__
20 CoreFoundation                 0x183928990 __CFRunLoopDoSources0
21 CoreFoundation                 0x183926690 __CFRunLoopRun
22 CoreFoundation                 0x183855680 CFRunLoopRunSpecific
23 GraphicsServices               0x184d64088 GSEventRunModal
24 UIKit                          0x1886ccd90 UIApplicationMain
25 SomeApp                        0x10021b67c main (main.m:42)
26 libdispatch.dylib              0x1833f68b8 (Missing)
```

It's said that this is a bug of NSURLSessionTask, which may resume downloading when app coming back from background even when you are not using it.

So my solution is

 * setup the NSURLSession with 

```objective-c
[NSURLSession sessionWithConfiguration:_configure]
```

 * add a method for NSObject when an unkown message is forwarded to.  
