---
layout: post
title: 如何做一个豆瓣购书单
---

我一直是豆瓣的粉丝，他们出的app在设计，定位和程序实现等层面上都在一个很高的水准之上。今天我的目标是豆瓣购书单，这是一款服务于拍书、查书、比价、购买备忘的移动应用。
先看看这个app都有哪些功能比较吸引人：

1.用手机拍书籍的ISBN条码

![douban preview 1](http://a4.mzstatic.com/us/r1000/104/Purple/ec/78/c1/mzl.svbanuuz.320x480-75.jpg)

2.展示对应图书的信息：

![douban preview 2](http://a3.mzstatic.com/us/r1000/060/Purple/f5/4f/9f/mzl.llnlxeve.320x480-75.jpg)

3.以及书的评价信息

![douban preview 3](http://a3.mzstatic.com/us/r1000/085/Purple/c8/b6/8b/mzl.gbzcjnrl.320x480-75.jpg)

4.比价和添加到购书单。

<hr/>

看着很cool对不对？扫描图书的条码，然后看看大家对这边书都是怎么看的，决定买了以后再看看有没有更便宜的卖家，或者收藏起来等有空再买。

那么，这些功能可以由一个人独立开发出来吗？——可以，而且很快:D

感谢开源社区，感谢豆瓣，他们为这个想法提供了捷径。

1.首先说扫描ISBN条码。

如何用程序来识别一张图片里的条码呢？开源项目[ZBar](http://zbar.sourceforge.net/index.html)帮我们解决了这个问题。ZBar是一个条码识别工具，能从图像中识别出条形码和QR码（就是一维码和二维码），支持多种形式的输入（图片文件，视频流），而且还是跨平台的（linux，windows，iOS…）。参照一下他提供的例子，就能轻松从iPhone的摄像头中读取出被扫描的条码了。

2.如何获取图书信息以及用户评价？

其实呢，豆瓣有提供获取图书信息的[API](http://www.douban.com/service/apidoc/reference/subject#获取书籍信息)。通过接口

```
GET http://api.douban.com/book/subject/isbn/{isbnID}
```
可以搜索到对应的ISBN号的书籍信息。我们可以看看返回结果

```
{
category: {
@scheme: "http://www.douban.com/2007#kind",
@term: "http://www.douban.com/2007#book"
},
db:tag: [
{
@count: 81,
@name: "片山恭一"
},
{
@count: 40,
@name: "日本文学"
},
{
@count: 21,
@name: "日本"
},
{
@count: 21,
@name: "小说"
},
{
@count: 12,
@name: "倘若我在彼岸"
},
{
@count: 11,
@name: "纯爱"
},
{
@count: 10,
@name: "日本小说"
},
{
@count: 10,
@name: "爱情"
}
],
title: {
$t: "倘若我在彼岸"
},
author: [
{
name: {
$t: "片山恭一"
}
}
],
summary: {
$t: "《倘若我在彼岸》是作者在《在世界中心呼唤爱》后的首部爱情小说集。片山恭一，学生时代通读了包括夏目漱石和大江健三郎在内的日本近现代文学全集，同时读了从笛卡尔、莱布尼茨到结构主义的欧洲近现代哲学，也读了马克思。自二十二三岁开始创作小说，《气息》、《世界在你不知道的地方运转》、《别相信约翰·列侬》等均为他的代表作。"
},
link: [
{
@rel: "self",
@href: "http://api.douban.com/book/subject/2023013"
},
{
@rel: "alternate",
@href: "http://book.douban.com/subject/2023013/"
},
{
@rel: "image",
@href: "http://img5.douban.com/spic/s9097939.jpg"
}
],
db:attribute: [
{
$t: "7543639130",
@name: "isbn10"
},
{
$t: "9787543639133",
@name: "isbn13"
},
{
$t: "倘若我在彼岸",
@name: "title"
},
{
$t: "193",
@name: "pages"
},
{
$t: "片山恭一",
@name: "author"
},
{
$t: "14.00元",
@name: "price"
},
{
$t: "青岛出版社",
@name: "publisher"
},
{
$t: "Paperback",
@name: "binding"
},
{
$t: "2007-1",
@name: "pubdate"
}
],
id: {
$t: "http://api.douban.com/book/subject/2023013"
},
gd:rating: {
@min: 0,
@numRaters: 170,
@average: "7.3",
@max: 10
}
}
```

看着还不错，对吧？书名，作者，出版社，日期，售价以及豆瓣评分都有了，还有其他的一些信息。当然，评价也是一样一样的.
具体的返回结果这里就不一一列举啦。

ok，今天先说到这里，下次再聊聊其他的东西吧。

谢谢观看。
