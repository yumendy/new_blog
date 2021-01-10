---
title: Python Challenge攻略-16
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-14 15:25:00
updated: 2018-05-31 00:24:17
---

*PIL 图像处理*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/return/mozart.html]()

这一关还是只有一张图片，一样的套路。先看源代码，发现一句提示“let me get this straight”。

它要得到一条直线，这样的话再看这张图片问题就变得似乎不那么难了，因为图片里有很多的粉红色的短横线，而且基本可以看出是每一行只有一段，放大图像可以看到每个短横线是5个像素，然后由两个白色的像素点包着。

那么我们需要做的就是把这些短横线对齐。看看会出现什么样的效果。

首先得确定这个短横线像素的表示方法。

在终端里用mode方法可以知道这张图片是P模式。这个就比较讨厌了，因为P模式是每张图片有一个色表，然后每个像素点的颜色用一个数字来代替，对应色表里的一个颜色，所以我们现在想知道这个紫色的横线到底是什么颜色的，我用的是最笨的办法就是如果相邻5个格颜色一样就输出这个颜色，取了众数就得到了195.

剩下的任务就简单了，用一个列表储存每一个紫色横线开始的像素点的坐标，然后再新的图像里统一填充到一个y坐标上就可以了。

```python
from PIL import Image

im = Image.open('mozart.gif','r')

w, h = im.size

po = []

for a in xrange(h):
	for b in xrange(w - 5):
		if im.getpixel((b,a)) == im.getpixel((b + 4,a)) == 195:
			po.append((b,a))

out = Image.new(im.mode,(w * 2, h), 0)

for item in po:
	st = w - item[0]
	for _ in xrange(w):
		out.putpixel((st + _, item[1]), im.getpixel((_, item[1])))

out.save('out.gif')
```

这样就可以得到最终的结果了。。。因为色表的原因，现在这个新的图像是是黑白的，但是这并不影响我们辨认里面的提示，得到下一关的地址：**romance**。
