---
layout: post
title: 如何在WEBVIEW加载APP内部资源
---

app开发中经常混合着html（webview）的开发，一来是因为html的方式比较灵活， 可以由服务端动态的控制代码， 二来html具有平台独立性， android／ios平台都能使用，降低了开发成本。

那么在app中， 如何打包html，css，js，以及，众多的图片资源，或者换句话说，在android和ios中的webview里，如何加载打包在app中的内部资源呢？

在android中，我们可以把web资源都放到assert目录里，

```
assets\html\
  index.html
  image1.png
  image2.jpg
```

然后里面的资源就能使用file:///前缀来访问了，如下所示

```html
<html>
<body>
  <img src="file:///android_asset/html/image1.png">
  <img src="file:///android_asset/html/image2.jpg">
</body>
</html>
```

加载webview：

```java
webView.loadUrl("file:///android_asset/html/index.html");
```

在iOS里，由于app在编译后会将所有的资源文件都打包到.app目录下，所以只要指定webview的baseURL就好了：

```objective-c
[webView loadHTMLString:htmlString baseURL:[[NSBundle mainBundle] bundleURL]];
```

然后在html中直接引用资源文件：

```html
<html>
<body>
  <img src="image1.png">
  <img src="image2.jpg">
</body>
</html>
```
