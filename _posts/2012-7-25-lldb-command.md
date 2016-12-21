---
layout: post
title: 记录一些使用LLDB调试的命令
---

有人说，现在iOS开发的门槛很低，随便学个几个月就能做出一个app来。据说过日子的第一版就是某个做金融的合伙人自学两个月倒腾出来的，真是让人亚历山大。然而，做一个app和做一个体验流畅，没有bug的app完全不是那么一回事。所以，今天这里分享一下一个酷毙程序员在大部分开发时间要做的事情——调试。

对于iOS开发来说，我相信绝大部分人使用的开发工具都是xcode。xcode本身集成了GDB工具，最近又升级到了LLDB。LLDB中的大部分命令和GDB是对应的。然后会有些许不同，完全的匹配表格可以参考[这里](http://lldb.llvm.org/lldb-gdb.html)。这里只记录几个常用的命令。

**命令“l”可以查看程序当前运行的位置**

```
(lldb) l
19 self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
20 if (self) {
21 _isLoading=NO;
22 // Custom initialization
23 }
24 NSLog(@"%@ initWithNib: %@", self, nibNameOrNil);
25 return self;
26 }
27
28 - (void)didReceiveMemoryWarning
29 {
```

**命令“bt”也能查看程序运行的调用栈**

```
(lldb) bt
* thread #1: tid = 0x1f03, 0x00090bea iBus`-[ABViewController initWithNibName:bundle:] + 42 at ABViewController.m:19, stop reason = breakpoint 1.1
    frame #0: 0x00090bea iBus`-[ABViewController initWithNibName:bundle:] + 42 at ABViewController.m:19
    frame #1: 0x0007be0d iBus`-[MapLineViewController initWithNibName:bundle:] + 109 at MapLineViewController.m:390
    frame #2: 0x011fccf1 UIKit`-[UIViewController init] + 49
    frame #3: 0x0001d461 iBus`-[ABAppDelegate application:didFinishLaunchingWithOptions:] + 1553 at ABAppDelegate.m:94
    frame #4: 0x01135386 UIKit`-[UIApplication _callInitializationDelegatesForURL:payload:suspended:] + 1292
    frame #5: 0x01136274 UIKit`-[UIApplication _runWithURL:payload:launchOrientation:statusBarStyle:statusBarHidden:] + 524
    frame #6: 0x01145183 UIKit`-[UIApplication handleEvent:withNewEvent:] + 1027
    frame #7: 0x01145c38 UIKit`-[UIApplication sendEvent:] + 68
    frame #8: 0x01139634 UIKit`_UIApplicationHandleEvent + 8196
    frame #9: 0x0346eef5 GraphicsServices`PurpleEventCallback + 1274
    frame #10: 0x021d2195 CoreFoundation`__CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE1_PERFORM_FUNCTION__ + 53
    frame #11: 0x02136ff2 CoreFoundation`__CFRunLoopDoSource1 + 146
    frame #12: 0x021358da CoreFoundation`__CFRunLoopRun + 2218
    frame #13: 0x02134d84 CoreFoundation`CFRunLoopRunSpecific + 212
    frame #14: 0x02134c9b CoreFoundation`CFRunLoopRunInMode + 123
    frame #15: 0x01135c65 UIKit`-[UIApplication _run] + 576
    frame #16: 0x01137626 UIKit`UIApplicationMain + 1163
    frame #17: 0x0001c74a iBus`main + 170 at main.m:16
    frame #18: 0x00002d75 iBus`start + 53
```

**查看程序当前状态下所有变量的值（包括参数和本地变量）**

```
(lldb) frame variable
(ABViewController *) self = 0x09853ae0
(SEL) _cmd = 0x06dc7ab3
(NSString *) nibNameOrNil = 0x00000000
(NSBundle *) nibBundleOrNil = 0x00000000
```

**查看某个变量的description**
```
(gdb) po item
{
id = 3;
name = "Blood Orange"; price = 110;
quantity = 1;
subtotal = 110;
}
```

我最常用的就是这几个命令了，更详细的介绍可以用help命令查看，这里就不罗列出来啦。当然了，调试不是单单使用几个命令就能搞得定的事情，这里需要更多的技巧或者经验，甚至还需要一些直觉。

在遇到程序莫名的崩溃时，使用bt命令打印出来的调用栈经常不能直接的发现错误的所在，甚至不知所云。例如下面这个例子：

```
                  Application received signal SIGSEGV
(null)
(
	0   CoreFoundation                      0x353058a7 __exceptionPreprocess + 186
	1   libobjc.A.dylib                     0x3672c259 objc_exception_throw + 32
	2   CoreFoundation                      0x35305789 +[NSException raise:format:] + 0
	3   CoreFoundation                      0x353057ab +[NSException raise:format:] + 34
```

遇到这种情况的话可以考虑考虑下面几种可能性：

* 检查一下是否向一个已经dealloc的对象发送了消息
* 检查一下异步的代码是否对一个已经dealloc的对象进行了操作
* 找到一些错误码，google或者stackoverflow一下

以上就是我的一些经验，欢迎大家来补充。
