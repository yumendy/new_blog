---
title: Python Challenge攻略-10
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-12 06:52:00
updated: 2018-05-29 06:30:37
---

*Python找规律*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/return/bull.html]()

这一关就是一个找规律然后写数字的题，没有什么难度，唯一蛋疼的就是那个找规律。。。其实我看了半天没看出他的规律是什么，然后就度娘了，发现其实就是一个简单的字符统计。然后问数列中第31个元素的长度，这个就很水了，随便写一个字符统计，循环调用然后添加进列表作为下一个的输入就ok了。

```python
def nextnum(num):
	temp = '0'
	result = ''
	counter = 0
	str_num = str(num)
	for ch in str_num:
		if ch != temp:
			result += str(counter)
			result += temp
			temp = ch
			counter = 1
		else:
			counter += 1
	result += str(counter)
	result += temp
	return int(result)

lst = [1]

for x in xrange(0,30):
	lst.append(nextnum(lst[x]))
print len(str(lst[30]))
```

最后得到的答案是：**5808**

（不禁想弱弱的黑一下C，这样的问题就蛋疼了吧。233333，人生苦短，我用Python）
