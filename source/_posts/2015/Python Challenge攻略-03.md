---
title: Python Challenge攻略-03
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-10 01:12:00
updated: 2018-05-31 21:31:36
---

*Python re包的使用*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/def/equality.html]()

这一关没有什么特别的地方，已经写的比较清楚了，就是找字符串，不用考虑都能确定用的一定是正则表达式。但是需要注意的是他的表述，“一个两边绝对被3个大写字母夹着的小写字母。”也就是说必须是这样形式的"xXXXaXXXx"只有满足这样形式的a才被认为是符合条件的字母。

那么只要按照这样的思路去编写正则表达式就可以了。

```python
import re
pattern = re.compile(r'(?<=[a-z][A-Z]{3})([a-z])(?=[A-Z]{3}[a-z])')
fp = open("data.in",'r')
text = fp.read()
fp.close()
m = re.findall(pattern,text)
print ''.join(m)
```

如此便可以得到下一关的url：**linkedlist**
