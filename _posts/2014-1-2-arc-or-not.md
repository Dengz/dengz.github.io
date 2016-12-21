---
layout: post
title: 如何确认当前环境是否使用了ARC
---

```objective-c
#if __has_feature(objc_arc) 
// ARC 
#else 
// No ARC 
#endif
```
