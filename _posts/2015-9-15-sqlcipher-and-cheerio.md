---
layout: post
title: Sqlcipher and Cheerio
---

官方版本的sqlite3是不支持数据库加密的，但是支持第三方通过插件的形式进行补充。Sqlcipher，正如其名称意义，可以对数据进行AES加解密。
![desc](https://www.zetetic.net/images/sqlcipher/showcase.png)

项目地址在[这里](https://github.com/sqlcipher/sqlcipher)


Cheerio是一个node插件，提供一种类jQuery的方式来解析html。这个插件可以用来替换jsdom插件，因为目前jsdom已经不好使了。

```
npm install cheerio
```
