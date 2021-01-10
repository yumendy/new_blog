---
title: Python Challenge攻略-12
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-13 02:27:00
updated: 2018-05-30 00:37:54
---

*Python文件处理*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/return/evil.html]()

这一关感觉是提示最少的一关了，还是只有一张图片，先把图片保存下来，发现一个小小的细节，就是这个图片的名字evil1.jpg。很容易想到的就是既然有了1，那么会不会有2和3或者其他的呢？

改一下地址，果然有2。一张图片上有一些字，写的是not .jpg _.gfx。那么就试一下，果然下载回来一个gfx的文件，但是这个文件格式根本不常见，度娘了一下也没有任何头绪，难道说就是设计者随便弄的一个吗？这个就不管了，然后再试试3，是一句话“no more evils”，大概意思就是再没有其他的图片了。再试试4的话发现是一幅无法打开的图片，F12查看资源，发现其实是一个文本文档，里面只有一句话“Bert is evil! go back!”这么看来的话应该是真的没有了，那么现在就该是拿这个evil2.gfx文件开刀了。

用文本编辑器打开发现是二进制文件，这样的话就不太好办了，用Python的终端先读几个字符试试吧。

```python
fp = open('evil2.gfx','r')
sub = fp.read(100)
print sub

# '\\xff\\x89G\\x89\\xff\\xd8PIP\\xd8\\xffNFN\\xff\\xe0G8G\\xe0\\x00\\r7\\r\\x00\\x10\\na\\n\\x10J'

```

其实看这个字符串。。。一下子感觉看到了很多熟悉的名字。比如GIF、PNG神马的。还有一个更要命的就是对称！每五个一组的对称！那么稍微处理一下刚才得到的东西：

```
\\xff \\x89 G   \\x89 \\xff
\\xd8 P    I   P    \\xd8
\\xff N    F   N    \\xff
\\xe0 G    8   G    \\xe0
\\x00 \\r   7   \\r   \\x00
\\x10 \\n   a   \\n   \\x10
```

好了，这个题目基本上没有什么问题了，可以确定这个文件就是几个图片拼起来的，而且是不同格式的图片，一共是五张。中间的三张是PNG、GIF、PNG，那么第一和第四章应该是什么呢？随手把一张jpg拖到文本编辑器里，看到开头的一串都是“FFD8 FFE0 0010”.接下来的工作就简单了，分离呗。纯文件操作了。

需要注意的是这个地方处理的都是二进制的文件，所以写入的时候一定要使用wb模式，不然就有的windows用户哭的了。

```python
fp = open('evil2.gfx','rb')

content = fp.read()

pic = ['jpg', 'png', 'gif', 'png', 'jpg']

for x in xrange(0,5):
	temp = open('%d.%s' % (x,pic[x]), 'wb')
	temp.write(''.join(content[x::5]))
	temp.close()

fp.close()
```

运行程序，发现输出了5张图片。第一张上是dis，第二张上是pro，第三张是port，第四张文件是损坏的，如果用Windows的图片查看是什么也看不出来的，直接拖到IE里，就能看见图片的内容了。是ional。第五张上面的文字被划掉了，可以不用管，这样的话就得到了最后的答案：**disproportional**.
