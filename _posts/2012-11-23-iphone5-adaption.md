---
layout: post
title: 如何让APP适配IPHONE5
---

iPhone5和iPod touch5的分辨率变成了960*1136, 不同于以往的长宽比例. 原有的app应该如何去适配这种比例呢?

1. 首先,你需要4.5版本以上的 [Xcode](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).
2. 设置一个4英寸大小的启动图片. 这个图片需要被命名为Default-568h@2x.png.
OK, 大功告成了.运行吧..

等等, 怎么布局有点乱? 好吧, 还有一些事情需要修改一下

把使用绝对像素定位的各种UIView换成Auto Layout.

怎么换?对于可以自动拉升布局的View,我们可以这样设置

```objective-c
view.autoresizingMask = UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleHeight;
```

如果需要对iPhone5实现不同布局, 我们只能判断一下设备情况了,方法如下

```objective-c
[[UIScreen mainScreen] bounds].size.height == 568 && UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPhone ? @"iPhone5" : @"others"
```

OK, 基本上这样就能解决这些个问题了
