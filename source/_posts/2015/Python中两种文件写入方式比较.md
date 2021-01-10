---
title: Python中两种文件写入方式比较
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-03-10 07:41:00
updated: 2018-06-01 19:28:49
---

*有关Python文件写入的相关问题*

<!-- more -->

在python中对一个文件的写入提供了两种方式，一种就是最常规的write方式，还有一种就是使用print>>方式。这两种方式的区别是什么呢？哪一个更快呢？

简单进行一个测试：

```python
# -*- coding: utf-8 -*-
from time import clock

def speedTest(func):
	def _deco():
		startTime = clock()
		func()
		endTime = clock()
		print "Cost time is " + str(endTime - startTime)
	return _deco

@speedTest
def method1():
	with open('a.txt','w') as fp:
		for x in xrange(1000000):
			fp.write("Hello world!\\n")
@speedTest
def method2():
	with open('b.txt','w') as fp:
		for x in xrange(1000000):
			print >> fp, "Hello world!"

print "write method:"
method1()
print 
print ">> method:"
method2()
```

最后的结果：

{% asset_img 1.png %}

显然是write方式更快一点。但是为什么呢？

在python的官方文档里面有这样的一段描述：

{% blockquote %}

print also has an extended form, defined by the second portion of the syntax described above. This form is sometimes referred to as “print chevron.” In this form, the first expression after the >> must evaluate to a “file-like” object, specifically an object that has a write() method as described above. With this extended form, the subsequent expressions are printed to this file object. If the first expression evaluates to None, then sys.stdout is used as the file for output.

{% endblockquote %}

可以看出在在使用>>的时候，python会先验证>>后是不是一个None，如果是的话就使用sys.stdout，这样的话多了验证工作所以速度就慢下来了喵~
