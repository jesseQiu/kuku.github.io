---
title: Gitbook
date: 2018-11-16 14:34:39
updated: 2018-11-16 14:34:39
categories: Tools
---

## 开源工具链
这个工具链 (GitBook) 是一个使用 Git 和 Markdown 来构建书籍的工具。它可以将你的书输出很多格式：PDF，ePub，mobi，或者输出为静态网页
GitBook 工具链是开源并且完全免费的，它的源码可以在 [GitHub](https://github.com/GitbookIO/gitbook) 上获取
与格式和工具链相关的问题被发表在 [github.com/GitbookIO/gitbook/issues](https://github.com/GitbookIO/gitbook/issues)

## GitBook.com
GitBook.com 是一个使用工具链来创建和托管书籍的在线平台 ([www.gitbook.com](https://www.gitbook.com/))

## 其他文档
开发文档 (API & 插件) 地址 [developer.gitbook.com](http://developer.gitbook.com/)。
企业版本的安装向导和手册的地址 [hep.enterprise.gitbook.com](http://help.enterprise.gitbook.com/)

## 注意
在使用 `gitbook pdf/epub/mobi` 生成电子文档时，需要先安装 [calibre](https://calibre-ebook.com/download_windows) 转换软件，否则会转换不成功。

## Questions
### `RangeError: Maximum call stack size exceeded`
默认的搜索插件建立的索引过大溢出,可以使用下面方法解决,使用 lunr 插件
```js
// book.json
// ...
"plugins": ["-lunr","autohome-fix-link","page-footer-copyright"],
"pluginsConfig": {
    "autohome-fix-link": {
        "link": "https://www.npmjs.com/"
    },
    "lunr": {
        "maxIndexSize": 100000000000000
   }
}
```
- [RangeError: Maximum call stack size exceeded #1009](https://github.com/GitbookIO/gitbook/issues/1009)

## 参考链接
- [gitbook git](https://github.com/GitbookIO/gitbook)
- [calibre windows](https://calibre-ebook.com/download_windows)
- [gitbook 中文文档](https://chrisniael.gitbooks.io/gitbook-documentation/content/format/markdown.html)
- [gitbook 实用配置及插件介绍](https://www.cnblogs.com/zhangjk1993/p/5066771.html)

