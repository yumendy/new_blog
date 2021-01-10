---
title: Python Challenge攻略-14
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-13 05:46:00
updated: 2018-06-01 19:29:36
---

*Python PIL图像操作*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/return/italy.html]()

这一关其实也是蛮水的，可以发现给的第二张图片比较有意思，其实是一个10000宽，高为1的图片，很显然是把一个100*100的图片经过了某种方式排列就变成了这样。

最容易想到的排列方式就是顺序排列，但是这个题目显然不会是那么简单，看到它给的另外一个算式的提示100*100 = (100+99+99+98) + (...。问题就变得简单了，如果以前有做过类似的题目应该可以想到那个螺旋形填数就是这样一个规律，这么看的话只用把像素点按这个规律再一次填进去就可以了。

```python
from PIL import Image

im = Image.open('wire.png','r')

pic = Image.new(im.mode,(100, 100))

index = 0

x = -1
y = 0

for l in xrange(100,0,-2):
	for t in xrange(l):
		x += 1
		pic.putpixel((x,y),im.getpixel((index,0)))
		index += 1
	for t in xrange(l - 1):
		y += 1
		pic.putpixel((x,y),im.getpixel((index,0)))
		index += 1
	for t in xrange(l - 1):
		x -= 1
		pic.putpixel((x,y),im.getpixel((index,0)))
		index += 1
	for t in xrange(l - 2):
		y -= 1
		pic.putpixel((x,y),im.getpixel((index,0)))
		index += 1
pic.save('pic.jpg')
```

我是用纯模拟写的，代码比较丑陋一些，但是不妨碍得出正确的结果~最后生成的一张图片是一只猫。那么过关的地址不言而喻了。cat。

但是输入cat之后，进入的页面并不是下一个题的，是这只猫的照片以及一句话，“and its name is uzi. you'll hear from him later.”

那么下一关的地址就是**uzi**了。
