---
title: Python Challenge攻略-01
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2014-11-12 18:11:00
updated: 2018-05-30 19:46:38
---

*Python string库相关内容。*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/def/map.html]()

这一关其实难度也不大，看一眼本子上写的内容立刻就知道了是一组最简单的替换密码，把字母依次向前推两个就可以得到正确的答案了。最基本的做法是用`ord`函数和`chr`函数通过ASCII码的加法可以简单的达到目的。

```python
def change(string):
    out = []
    for item in string:
        if ord(item) <= ord('x') and ord(item) >= ord('a'):
            out.append(chr(ord(item) + 2))
        elif item == 'y':
            out.append('a')
        elif item == 'z':
            out.append('b')
        else:
            out.append(item)
    return ''.join(out)
    
src = raw_input()

print change(src)
```

但是！！！

真的需要这么多代码来做这一件事情吗？？

我们用的可是Python。。。

一定是姿势不对~~~

没错。。。。。

其实正确的做法应该是这样的：

```python
import string

text = raw_input()

table = string.maketrans(string.ascii_lowercase,string.ascii_lowercase[2:]+string.ascii_lowercase[:2])

print(string.translate(text,table))
```

没错！！真的只需要四行就可以搞定这个问题了，但是这些代码又是什么意思呢？

一起来看看吧~

先说说string里的一些常量吧：

*string.ascii_letters*：一个包含小写字母和大写字母的字符串。

*string.ascii_lowercase*：一个只包含小写字母的字符串。

*string.ascii_uppercase*：一个只包含大写字母的字符串。

*string.digits*：一个包含0~9数字的字符串。

在这个问题中，用到了两个特殊的string的方法：

第一个是`string.maketrans`，第二个是`string.translate`。它们分别是做什么的呢？

逐个来说吧：

`string.maketrans`

函数原型：

```python
string.maketrans(from, to)
```

函数的功能就是创建了一个从字符到字符的映射表，为后续使用translate()提供基础。

第一个参数是原始字符串，第二个参数是目标字符串。

在这个问题中，将字符向前移位三个，所以原始字符串是从a到z的字符，所以可以直接用string.ascii_lowercase来作为第一个参数。

第二个参数可以通过对`string.ascii_lowercase`常量的切片和组合来完成。这样就构建成了一个字符的映射表。

接下来的工作就是对字符串的翻译。

我们可以使用`translate`函数。

函数原型：

```python
string.translate(s, table[, deletechars])
```



接受三个参数，其中第三个参数是可选的。

第一个参数为需要解密的字符串，第二个参数是映射表，第三个参数是需要删除的字符表。

返回值为翻译完成的字符串。

根据题目的提示，对url翻译可以得到下一关的地址：**ocr**。
