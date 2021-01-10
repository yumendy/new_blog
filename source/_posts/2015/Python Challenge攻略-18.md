---
title: Python Challenge攻略-18
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-16 03:52:00
updated: 2018-05-29 14:59:52
---

*Python difflib*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/return/balloons.html]()

这一关的提示是让你说出两张图片的不同，而且是很基础的不同，那就是亮度了。

改地址：http://www.pythonchallenge.com/pc/return/brightness.html

然后就可以进到一个完全一样的页面里了，但是源码里的提示变了。

<!-- maybe consider deltas.gz -->

改地址就可以下载到一个deltas的压缩包。里面是一个txt的文件。打开一看很明显文件是被分割成了两部分。而且左右两侧大部分相同，只有个别行是左边有右边没有，或者右边有左边没有。

处理这个问题一下子没有想到太好的办法，就去Python官方文档的标准库页面找思路，ctrl+F了一下deltas。。就出现了一个结果。。是一个叫做difflib的东西，这就没有什么好解释的了吧。。直接查文档，然后发现这个库的作用就是用来找差异的。输入两个片段，然后逐行比较，将差异分为三个类型，两个片段都有，只有第一个片段有和只有第二个片段有。

因为需要两个输入，所以这样的话，最容易想到的，就是先分离文档，将原来的一个文件分为左右两个部分。

```python
import difflib

src = open('delta.txt','rb')

dst1 = open('dst1.txt','wb')
dst2 = open('dst2.txt','wb')

for line in src.readlines():
	dst1.write(line[:53] + '\\n')
	dst2.write(line[56:])

dst1.close()
dst2.close()
src.close()
```

这样的话就输出了两个文本文件，把原来的一个分成了两部分，然后紧接着可以想到的就是把它扔到这个库里跑一下看看会出现什么结果。

```python
sec1 = open('dst1.txt','rb')
sec2 = open('dst2.txt','rb')
diff = open('diff.txt','wb')

text1 = sec1.read().splitlines(1)
text2 = sec2.read().splitlines(1)

sec1.close()
sec2.close()

d = difflib.Differ()

re = list(d.compare(text1,text2))

for line in re:
	diff.write(line)

diff.close()
```

打开diff.txt看了一下就知道是怎么回事了，发现两个相同的部分，和只有左边有的，以及只有右边有的，都是以“89 50 4e 47 0d 0a 1a 0a 00”这样的串开头的，不禁想到的就是之前那5张图片的那一关，估计手法差不多吧。

百度一下这串数字。。果然是PNG。也就是说这是三幅独立的PNG。那么问题就好办了，分离。

但是特别要提醒的一点。。也是卡了我好久的。。。就是这个文件不是一个二进制文件。。也就意味着分离之后也不是一个png里面的这些数字都是人为搞成16进制的，也就是说这一堆还是acsii。。。擦。。别问我是怎么知道的。。。当我用notepad打开这个文件发现看到的东西和我用sublime打开看到的东西是一样的之后。。。。简直不忍直视。

C的标准库里有一个字符串转数值的，所以用Python也必须有。

翻了翻标准库，果然有一个库是用来干这个事情的。binascii。

```python
for line in re:
	if line[:2] == '  ':
		temp1 += line[2:-1].replace(" ",'')
	elif line[:2] == '+ ':
		temp2 += line[2:-1].replace(" ",'')
	elif line[:2] == '- ':
		temp3 += line[2:-1].replace(" ",'')


pic1 = open('pic1.png','wb')
pic1.write(binascii.unhexlify(temp1))
pic1.close()
pic2 = open('pic2.png','wb')
pic2.write(binascii.unhexlify(temp2))
pic2.close()
pic3 = open('pic3.png','wb')
pic3.write(binascii.unhexlify(temp3))
pic3.close()
```

这样的话就分离出三幅图片来。

里面的内容是：

../hex/bin.html

butter

fly
