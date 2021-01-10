---
title: Python Challenge攻略-07
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-11 03:11:00
updated: 2018-05-28 23:08:35
---

*Python经典图像处理库PIL的使用*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/def/oxygen.html]()

如果以前有接触过解谜类的游戏的话，这一个其实一下子就可以看出解题的思路来。但是问题就在于如果用一个Python的方法来做就需要想想了。

看到的是一张图片，上面有一个灰度条。作为一个weber，对灰度条还是比较敏感的，为什么？灰度条的rgb是相同的，同时它是一个0~255间的值。

看见255顿时感觉其实脑洞可以很大。传递信息的话。。当然还有一个东西就是ASCII了。。这么想下来的话就豁然开朗了。

处理图片自然要用到PIL。之前没有接触过，所以花了一晚上的时间去查PIL的资料，发现还是用起来非常蛋疼。

不管了，能用就可以。

先简单量了一下像素，发现整个条大概是609像素的样子，每个格子的宽度是7个像素。其余的就是格子距离上边界大概是45个像素。那么就可以去提取每一个格子的灰度值了。

注意的是代码里需要用到的几个函数和接口，open是打开一个图像对象。crop是获取图片的一个局部区域。getdata获取到的是一个一维数组，里面是每一个像素的信息，用一个元组表示，这一张图片是用一个四元组（R,G,B,A）表示像素信息的。

```python
from PIL import Image

im = Image.open("oxygen.png")

box = (0,44,607,45)

part = im.crop(box)

result_num = []

for i in xrange(0,607,7):
	temp = part.getdata()[i]
	result_num.append(temp)

print ''.join(map(lambda x: chr(x[0]),result_num))
```

好了，看看我们得到了什么，其实就是一句话：“smart guy, you made it. the next level is [105, 110, 116, 101, 103, 114, 105, 116, 121]”

这还有什么疑问咩？再加两行代码：

```python
lst = [105, 110, 116, 101, 103, 114, 105, 116, 121]

print ''.join(map(chr,lst))
```

输出结果：**integrity**。

看了官方的解答之后。。。发现一个更简洁的解法。。。

```python
import PIL.Image
print PIL.Image.open('oxygen.png').tostring()[108188:110620:28]
```

没错。。。就酱。。
