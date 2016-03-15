title: win下安装Scrapy
date: 2016-02-21 19:30:47
updated:
tags:
- Python
- 爬虫
categories:
- Python
permalink: install-scrapy-win
---

> 在win下配置开发环境是一件很烦的事情，这也是我为什么基本不再win下写代码的原因。但是由于某些原因，不得不在win下进行开发，所以我现在很烦躁。。。

---

### 在windows下安装Scrapy需要做一点准备工作

1. 因为需要安装pywin32，所以需要准备好win下的pywin32的安装包。—— [pywin32](http://sourceforge.net/projects/pywin32/)
2. 除了pywin32之外，还需要安装lxml，而在win下安装lxml时总是会出现找不到 `vsvarsall.bat` 的错误。因此安装lxml也是需要自己下载MS installer。然后使用easy_install来安装： `easy_install "c:/lxml_installer.exe"`  —— 下载：[lxml3.5.0](https://pypi.python.org/pypi/lxml/3.5.0)
3. 安装上述模块成功之后就可以使用pip安装Scrapy了！—— `pip install Scrapy`

---

> 在安装完Scrapy之后就可以使用了，具体可以参考scrapy的[文档](http://doc.scrapy.org/en/latest/index.html)

> 最后，Ubuntu大法好，退win保平安。
