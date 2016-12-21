---
layout: post
title: Build dylib project on Xcode
---

Xcode默认是不能在iOS上创建dylib工程的，一般的解决办法是修改Xcode的配置文件

```
/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Specifications/iPhoneOSProductTypes.xcspec  
/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Specifications/iPhoneOSPackageTypes.xcspec
```

如果项目需要多人协作，那么每个人都得修改自己Xcode。

Facebook提供了一个[解决方案](https://github.com/facebook/clang-as-ios-dylib)，在编译阶段修改替换CC和LD命令，在Xcode调用clang之前先替换掉部分编译选项，从而编译成功。

问题是这个开源项目只支持在模拟器上编译，而在手机上跑不通。好东西不私藏，我在上面做了些修改，添加了对手机的支持。[地址在这里](https://github.com/Dengz/clang-as-ios-dylib)。
