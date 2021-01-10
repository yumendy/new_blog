---
title: Python Challenge攻略-17
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-15 06:18:00
updated: 2018-05-30 04:13:58
---

*Python urllib和cookielib模块*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/return/romance.html]()

这一关应该说是做了这么久最蛋疼的一关了吧。。。

真的是非常的冗长，不说别的。。一看图就知道是什么了，cookies。

F12然后刷新页面，在cookie里看到了一行字：info=you+should+have+followed+busynothing...

然后再看看下面的配图。。是第四关的配图，那么就只好去第四关看看cookie是什么了。

在这个页面http://www.pythonchallenge.com/pc/def/linkedlist.php上，cookie还是没有变。

刚才的提示是followed busynothing。所以把上面的url改一下http://www.pythonchallenge.com/pc/def/linkedlist.php?busynothing=12345 看看发生了什么。

Response cookie出现了值，是一个字母，然后当你继续访问的时候，字母在变。

这下好办了，收集这些值：

```python
import urllib2
import cookielib

jar = cookielib.CookieJar()
handler = urllib2.HTTPCookieProcessor(jar)
opener = urllib2.build_opener(handler)

baseurl = 'http://www.pythonchallenge.com/pc/def/linkedlist.php?busynothing='
num = '12345'

info = []

for _ in xrange(118):
	content = opener.open(baseurl + num).read()
	num = content.split()[-1]
	info.append(jar._cookies.values()[0]['/']['info'].value)

print ''.join(info)
```

输出的是一大段字符串：

```
BZh91AY%26SY%94%3A%E2I%00%00%21%19%80P%81%11%00%AFg%9E%A0+%00hE%3DM%B5%23%D0%D4%D1%E2%8D%06%A9%FA%26S%D4%D3%21%A1%EAi7h%9B%9A%2B%BF%60%22%C5WX%E1%ADL%80%E8V%3C%C6%A8%DBH%2632%18%A8x%01%08%21%8DS%0B%C8%AF%96KO%CA2%B0%F1%BD%1Du%A0%86%05%92s%B0%92%C4Bc%F1w%24S%85%09%09C%AE%24%90
```

一看就是经过转意的，那么就得转回来。在urllib库里有相应的转意函数：

```python
import urllib
message = ''.join(info)
message = urllib.unquote_plus(message)
print message
```


再看看输出：

```
'BZh91AY&SY\\x94:\\xe2I\\x00\\x00!\\x19\\x80P\\x81\\x11\\x00\\xafg\\x9e\\xa0\\x00hE=M\\xb5#\\xd0\\xd4\\xd1\\xe2\\x8d\\x06\\xa9\\xfa&S\\xd4\\xd3!\\xa1\\xeai7h\\x9b\\x9a+\\xbf`"\\xc5WX\\xe1\\xadL\\x80\\xe8V<\\xc6\\xa8\\xdbH&32\\x18\\xa8x\\x01\\x08!\\x8dS\\x0b\\xc8\\xaf\\x96KO\\xca2\\xb0\\xf1\\xbd\\x1du\\xa0\\x86\\x05\\x92s\\xb0\\x92\\xc4Bc\\xf1w$S\\x85\\t\\tC\\xae$\\x90'

```

一种莫名的熟悉感让我想起了之前做过的bz2，于是果断decode。

```python
print message.decode('bz2')
```

如果说感觉这就是结果的话。。。真的高兴的有点太早了：

{% blockquote %}
is it the 26th already? call his father and inform him that "the flowers are on their way". he'll understand.
{% endblockquote %}

这一句话的信息量略微有点大。。。26th是之前那一关的提示。那么这个地方的他指的就是莫扎特了，百度一下莫扎特的父亲是Leopold。

改网站。。404。。还是不对，在看看上面的提示：call his father。

WTF!!

这是又回到打电话的那一关了吗？好吧。。

```python
import xmlrpclib
proxy = xmlrpclib.ServerProxy('http://www.pythonchallenge.com/pc/phonebook.php')
print proxy.phone('Leopold')
```

果然。。。

555-VIOLIN

改地址：出现了这样一个提示：

{% blockquote %}
no! i mean yes! but ../stuff/violin.php.
{% endblockquote %}

再改。。好了终于有东西出来了，但是我发现这不是下一个题的地址，因为图片上没有标注题号。。。再看看网页的标题：it's me. what do you want?

好吧。。。。还有东西不对。。。再回头看看上面的提示。。inform him that "the flowers are on their way".

也就是说在访问的时候需要给服务器发送这个消息，这一关又主要是cookie，那就是需要构造这样一个cookie然后去访问这个网址了吧。。。

```python
jar = cookielib.CookieJar()
handler = urllib2.HTTPCookieProcessor(jar)
opener = urllib2.build_opener(handler)
jar._cookies.values()[0]['/']['info'].value = 'the+flowers+are+on+their+way'
print opener.open('http://www.pythonchallenge.com/pc/stuff/violin.php').read()
```

看看返回了什么：

```html
<html>
<head>
  <title>it's me. what do you want?</title>
  <link rel="stylesheet" type="text/css" href="../style.css">
</head>
<body>
	<br><br>
	<center><font color="gold">
	<img src="leopold.jpg" border="0"/>
<br><br>
oh well, don't you dare to forget the balloons.</font>
</body>
</html>
```

QAQ。。终于。。。找到答案了。。。**balloons**。
