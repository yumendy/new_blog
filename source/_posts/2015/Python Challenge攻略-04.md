---
title: Python Challenge攻略-04
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-10 01:41:00
updated: 2018-06-02 02:26:11
---

*Python urllib*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/def/linkedlist.php]()

这一关是非常有意思的一关，点击一下图片就可以进入一个页面写着“and the next nothing is xxxxx”。其中xxxxx是一个数字，在看一眼url就知道是什么意思了，就是这样不断的去点url它总会给出一个提示的，但是这样繁杂的工作如果让人去做的话就真的是无比蛋疼了，Python一直号称是在web编程方面极具有特色的一种语言，那么处理这样的问题应该也是非常简单的。

遇到网络问题首先想到的就是Python标准库里的urllib和urllib2.两个基本的库。

urllib可以将一个网页以文件的形式打开，方便了用户以文件的形式读取内容。urllib2则提供了更为强大的各种功能，包括cookies和session等。

在这个题目里简单的使用urllib就可以搞定了。

```python
import urllib
url = 'http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing='
num = '8022'
while(1):
    line = urllib.urlopen(url+num).readline()
    num = line.split()[-1]
    print line
```

很快就得到了下一关的提示：**peak.html**
