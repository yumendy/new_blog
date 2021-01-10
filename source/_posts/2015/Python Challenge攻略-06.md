---
title: Python Challenge攻略-06
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-10 10:20:00
updated: 2018-05-31 12:42:00
---

*python zip相关操作*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/def/channel.html]()

其实感觉真正是从这一关开始变得越来越有意思了。

看源代码。发现了关键的提示zip。。。

结合上一关的经验，想到的就是应该和一个zip文件有关，那么就先得得到这个文件。改url，把拓展名改为zip果然可以拉下来一个文件。zip的压缩包，打开之后发现是一大堆数字命名的txt，顿时想到了前几关的方法。。。好评的是给你了一个readme.txt的文件，并且告诉你了开始的文件。

那么就好办了。。。

```python
num = '90052'
failname = num + '.txt'
while 1:
    fp = open(failname,'r')
    S = fp.readline()
    num = S.split()[-1]
    failname = num + '.txt'
    print S
    fp.close()
```

发现运行到最后出现这样一句话“Collect the comments.”

那么问题就来了。。。注释。。哪里有注释呢？又想了想。。压缩包好像有注释这样的东西，那不就意思是要对压缩包进行一定的操作嘛~~所以果断去翻Python的文档。在标准库中找到了一个zipfile的库。。。专门用来处理zip文件。在页面里搜索comment，发现每一个压缩包内文件对象都会有一个ZipInfo对象，在这个对象中有一个属性就是comment。顿时感觉看到了曙光。

改改刚才的代码接着用：

```python
import zipfile

num = '90052'
failname = num + '.txt'
myzip = zipfile.ZipFile('channel.zip','r')


while 1:
	print myzip.getinfo(failname).comment,
	fp = open(failname,'r')
	S = fp.readline()
	num = S.split()[-1]
	failname = num + '.txt'
	fp.close()

myzip.close()
```

看看接下来输出了神马= =b

```

* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
* *                                                                                                                         * * 
* *       O O         O O         X X             Y Y Y Y         G G         G G     E E E E E E   N N             N N     * * 
* *       O O         O O     X X X X X X       Y Y Y Y Y Y       G G       G G       E E E E E E     N N         N N       * * 
* *       O O         O O   X X X     X X X   Y Y Y       Y Y     G G   G G           E E               N N     N N         * * 
* *       O O O O O O O O   X X         X X   Y Y                 G G G               E E E E E           N N N N           * * 
* *       O O O O O O O O   X X         X X   Y Y                 G G G               E E E E E             N N             * * 
* *       O O         O O   X X X     X X X   Y Y Y       Y Y     G G   G G           E E                   N N             * * 
* *       O O         O O     X X X X X X       Y Y Y Y Y Y       G G       G G       E E E E E E           N N             * * 
* *       O O         O O         X X             Y Y Y Y         G G         G G     E E E E E E           N N             * * 
* *                                                                                                                         * * 
* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
  * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
```

hockey？

输入之后出现一个提示“it's in the air. look at the letters.”

看字母。。。

发现组成每一个字母的字母都不一样，那么新的提示就来了：**oxygen**。

就是它了！
