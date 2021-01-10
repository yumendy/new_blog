---
title: Python Challenge攻略-19
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-16 07:48:00
updated: 2018-06-01 19:30:46
---

*Python wave 音频处理库*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/hex/bin.html]()

又一次看到了Python的强大，真的。

这一关怎么说呢。。。挺无语的。

先看看源码，里面是一个用base64加了密的音频文件。

把源码拷贝出来解密就好。

```python
wav = open('indian.wav','wb')

temp = open('temp','r')

wav.write(temp.read().decode('base64'))

wav.close()
temp.close()
```

然后就可以听这段音频文件了，但是发现很遗憾的是，只能听清楚一个sorry。然后改url发现并不是下一关的地址。

只有一句提示："what are you apologizing for?"

这个就比较糟糕了。去翻翻Python的标准库。。居然还有处理音频用的库。。想想也是醉了。。还有Python干不了的事情嘛？

于是用这个库打开音频看了一下这个音频的各项属性。

发现对于一个音频主要有这么几个属性：频率、声道数、采样宽度，采样数。

其中很有意思的就是这个采样宽度是以字节计数的，而且是两个字节！！为什么是两个字节。。。又是一个偶数。这是黔驴技穷了的节奏咩？

依旧想到的是。。扒开看看。。

不过因为是音频，所以需要注意的是，扒开的时候音频的时长实际上是缩短了一半，所以频率也得缩短一半，否则就没法听了。

```python
import wave

wav = wave.open('indian.wav')
out1 = wave.open('out1.wav','w')
out2 = wave.open('out2.wav','w')

out1.setparams(wav.getparams())
out2.setparams(wav.getparams())

out1.setframerate(wav.getframerate() / 2)
out2.setframerate(wav.getframerate() / 2)

for i in range(wav.getnframes()):
	data = wav.readframes(1)
	out1.writeframes(data[0])
	out2.writeframes(data[1])

out1.close()
out2.close()
wav.close()
```

两个文件，发现第二个文件还是之前听到的声音，但是第一个文件已经变成了可以听到其他内容的东西了。

仔细听就可以听到里面的话是“You are an idiot.”

关键词是idiot。

所以就可以去改地址了，出现的提示是："Now you should apologize..."
