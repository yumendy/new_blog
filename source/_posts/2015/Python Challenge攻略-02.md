---
title: Python Challenge攻略-02
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-06 05:58:00
updated: 2018-05-28 23:22:36
---

*Python频繁元素统计*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/def/ocr.html]()

这一关实际上非常简单，在屏幕提示的“recognize the characters. maybe they are in the book, but MAYBE they are in the page source.”这一句话稍微一琢磨就可以知道，此处的source指的是源代码。那么直接查看网页源码，果不其然有一大段注释，是由各种符号组成的，有一句提示是find rare characters in the mess below。就是说在里面找出出现频率低于平均值的字符。放眼望去一片标点符号。那么最简单的办法就是直接筛出来英文字母了。把文段内容全部拷贝到一个“in.data”的文件中，然后执行下面的代码就可以轻松的得到最后的答案了。

```python
outstring = ''
fp = open('in.data','r')
    for line in fp:
        for letter in line:
            if letter >= 'a' and letter <= 'z':
                outstring += letter
fp.close()
print outstring
```

如果需要更简单的解法，那么可以使用`filter`函数来实现。

```python
import string
fp = open('in.data','r')
print filter(lambda x: x in string.letters, fp.read())
fp.close()
```

关于filter函数：

`filter(function, sequence)`


对sequence中的item依次执行function(item)，将执行结果为True的item组成一个List/String/Tuple（取决于sequence的类型）返回

但是不得不承认的是，这是一种偷懒的做法，如果真的要去统计具体的字符频率的话可以使用字典的方法。这样可以减少难度。

```python
outstring = ''
dic = {}

fp = open('in.data','r')

for line in fp:
    for letter in line:
        if letter in dic:
            dic[letter] += 1
        else:
            dic[letter] = 1
        if letter >= 'a' and letter <= 'z':
            outstring += letter
fp.close()
lst = dic.items()
lst.sort(key = lambda x : x[1])
for x in lst:
    print x
print outstring
```

得到下一关的地址：**equality**
