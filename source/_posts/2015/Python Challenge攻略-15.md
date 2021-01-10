---
title: Python Challenge攻略-15
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-13 06:24:00
updated: 2018-06-01 19:31:32
---

*Python datetime模块*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/return/uzi.html]()

其实本来今天不想继续做了的，结果发现这一关。。真的是一大水关，所以就决定做完这一关。

一看这个日历就知道是要求年份，果断是datetime库，需要注意的一点可能就是从右下角可以看出2月有29天，那么我们需要找的就是最后一位年份的数字是6，而且是闰年的年份。

```python
from datetime import date

for year in xrange(1996,1000,-20):
	if date(year,1,1).isoweekday() == 4:
		print year,
```

符合条件最近的是1996，由于是闰年所以步长可以设置为-20.

1976 1756 1576 1356 1176

最后得到的结果就是这些数字。

看源码，里面有一句提示“he ain't the youngest, he is the second ”。那么我们需要的年份就是1756了。

但是这个不是最后的答案。

这个题目的标题是whom，那么答案应该是和一个人有关，而且还有一点很重要的就是还有一个提示信息没有用。

“todo: buy flowers for tomorrow”

而日历里圈出来的日期是1月26号。

综合这些信息，就可以知道，我们需要得到一个人的信息，而1756-1-27是与这个人相关的日期。

百度一下，莫扎特诞生于1756-1-27.那么最后的答案就是**mozart**。
