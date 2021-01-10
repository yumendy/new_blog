---
title: Python Challenge攻略-08
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-12 03:24:00
updated: 2018-06-01 03:13:13
---

*Python bz2 模块*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/def/integrity.html]()

无语的一关，直接看源码，就看到了用户名和密码。想起来Python课的时候有一个实验就是和编码有关的，不过都是utf-8和unicode或者gbk之间转换的。但是这个一看就不是。隐隐有一种蛋疼的感觉，不过发现字符串的开头都是BZh91AY&SY开头的。记得以前学编码的时候看过一些资料说这样的开头都是一种固定的编码方式编出来的，就是bz2.这样的话问题就变得无比简单了。

```python
print 'BZh91AY&SYA\\xaf\\x82\\r\\x00\\x00\\x01\\x01\\x80\\x02\\xc0\\x02\\x00 \\x00!\\x9ah3M\\x07<]\\xc9\\x14\\xe1BA\\x06\\xbe\\x084'.decode('bz2')
print 'BZh91AY&SY\\x94$|\\x0e\\x00\\x00\\x00\\x81\\x00\\x03$ \\x00!\\x9ah3M\\x13<]\\xc9\\x14\\xe1BBP\\x91\\xf08'.decode('bz2')
```

两行代码搞定。

得到用户名：**huge**
密码：**file**

