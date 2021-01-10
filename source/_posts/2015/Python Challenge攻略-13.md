---
title: Python Challenge攻略-13
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-01-13 03:29:00
updated: 2018-06-01 19:31:57
---

*Python xmlrpclib使用*

<!-- more -->

关卡地址：[http://www.pythonchallenge.com/pc/return/disproportional.html]()

这一关从源代码上貌似看不出什么东西来，提示也只有一句：“phone that evil ”。

但是点了一下图片里的5发现打开了一个页面。

其实是返回了一个XML的文件，这一下就知道了，这个server不是一个网页的服务器，应该是一个通过XML调用完成信息交互的服务器。那么问题一下子就好办多了，当时上服务计算课程的时候讲了这样的处理办法，在Python里已经有非常方便的内建的类库了。就是xmlrpclib，这是一个可以作为XML-RPC客户端用的简单的类库。功能也足够强大了。

这一关的话可以就可以只在终端里面进行，可以不用写脚本了。

```python
import xmlrpclib
proxy = xmlrpclib.ServerProxy('http://www.pythonchallenge.com/pc/phonebook.php')
```

导入标准库并且实例化一个远程的服务对象。

接着就可以用listMethods方法去查看远程的接口了。

```python
proxy.system.listMethods()

# ['phone', 'system.listMethods', 'system.methodHelp', 'system.methodSignature', 'system.multicall', 'system.getCapabilities']
```

果然不出所料。。就是一个标准的XML-RPC服务器，有一个可供调用的接口phone，然后就是查询一下这个接口的说明和调用方法：

```python
proxy.system.methodHelp('phone')

# 'Returns the phone of a person'

proxy.system.methodSignature('phone')

# [['string', 'string']]
```

接受的参数是字符串，那么问题就好办了，直接调用就ok了，但是新的问题来了，我们该往里面输入什么样的字符串呢？想想这个题目里貌似还有一个条件没有用到，就是一开始的那一个提示，“phone that evil ”。问题来了，这一句话怎么理解。

如果是像我一样刚刚做完第12题的话，一定还记得最后一个不是图片的提示吧：“Bert is evil! go back!”。

好了，这下就没有任何的疑问了。

```python
proxy.phone('Bert')

# '555-ITALY'
```

最终的答案就是**italy**。
