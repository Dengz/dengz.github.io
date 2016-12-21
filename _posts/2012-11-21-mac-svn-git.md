---
layout: post
title: 解决MAC OS X 10.8下SVN和GIT的问题
---

最近又升级了一台机器到 Mac OS X 10.8 – Mountain Lion, 又遇到老问题: svn和git不能用..

网上有很多文章都推荐去下载Xcode的[Command Line Tools](https://daw.apple.com/cgi-bin/WebObjects/DSAuthWeb.woa/wa/login?appIdKey=d4f7d769c2abecc664d0dadfed6a67f943442b5e9c87524d4587a95773750cea&path=%2F%2Fdownloads%2Findex.action), 已经安装了Xcode的同学, 也可以通过

```
Xcode > Preferences > Downloads > Command Line Tools > Install
```

这个方法安装.
当然,如果系统升级到10.8之前已经安装了最新的Xcode,其实也有一些简单办法的:

```
sudo ln -s /PathToXcode/Xcode.app/Contents/Developer/usr/bin/svn /usr/bin/svn
sudo ln -s /PathToXcode/Xcode.app/Contents/Developer/usr/bin/make /usr/bin/make
sudo ln -s /PathToXcode/Xcode.app/Contents/Developer/usr/bin/SetFile /usr/bin/SetFile
```
