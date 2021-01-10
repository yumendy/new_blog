---
title: Python多进程编程中进程池锁共享问题
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2016-08-29 06:39:15
updated: 2018-06-02 05:30:19
---

Python多进程编程中，资源锁的问题

进程池可以使我们的编程变得非常简单，但是同时进程池的使用也会造成一些麻烦，比如对于共用锁的处理等。

这篇文章主要通过两种方法解决共享资源锁的问题：

* 通过Manager对象
* 通过初始化时传递

<!-- more -->

python多进程编程使用进程池非常的方便管理进程，但是有时候子进程之间会抢占一些独占资源，比如consol或者比如日志文件的写入权限，这样的时候我们一般需要共享一个Lock来对独占资源加锁。lock作为一个不可直接打包的资源是没有办法作为一个参数直接给Pool的map方法里的函数传参的。为了解决这个问题，有两种解决方法，一种是使用多进程的管理器Manager()，并使用偏函数的办法传递对象Manager.Lock()。第二种是在进程池创建时传递multiprocessing.Lock()对象。

下面以一个具体的栗子来说明。

比如我现在有一个数据列表我想通过多进程的方式将里面的数据发送到指定的API并且在日志文件中记录每次请求所用的时间。

我们最容易想到的解决办法就是把锁作为一个参数传进去：

```python
from multiprocessing import Pool, Lock
import urllib2
from time import clock
from functools import partial

def send_request(lock, data):
	api_url = 'http://api.xxxx.com/?data=%s'
	start_time = clock()
	print urllib2.urlopen(api_url % data).read()
	end_time = clock()
	lock.acquire()
	whit open('request.log', 'a+') as logs:
		logs.write('request %s cost: %s\
' % (data, end_time - start_time))
	lock.release()

if __name__ == '__main__':
	data_list = ['data1', 'data2', 'data3']
	pool = Pool(8)
	lock = Lock()
	partial_send_request(send_request, lock=lock)
	pool.map(partial_send_request, data_list)
	pool.close()
	pool.join()
```

在这样的情况下，lock作为一个不可直接打包的资源是没有办法作为一个参数直接给Pool的map方法里的函数传参的。

会出现一个运行时错误：

{% blockquote %}
Runtime Error: Lock objects should only be shared between processes through inheritance.
{% endblockquote %}

根据一开始的思路我们可以把代码改成下面的样子：

第一种思路，使用Manager。

send_request函数不用改变，只改变main中的内容：

```python
if __name__ == '__main__':

	from multiprocessing import Manager

	data_list = ['data1', 'data2', 'data3']
	pool = Pool(8)
	manager = Manager()
	lock = manager.Lock()
	partial_send_request(send_request, lock=lock)
	pool.map(partial_send_request, data_list)
	pool.close()
	pool.join()
```

这是第一种方法，但是对于仅仅需要一个日志写入锁就用一个Manager显的十分重了。这种方式其实是需要一个专门的进程去处理Manager服务。所有的加锁和释放锁的操作都是通过IPC传递给Manager服务的。

第二种解决思路就是通过initializer参数在Pool对象创建时传递Lock对象。这种方式将Lock对象变为了所有子进程的全局对象。

代码可以作如下修改：

```python
def send_request(data):
	api_url = 'http://api.xxxx.com/?data=%s'
	start_time = clock()
	print urllib2.urlopen(api_url % data).read()
	end_time = clock()
	lock.acquire()
	whit open('request.log', 'a+') as logs:
		logs.write('request %s cost: %s\n' % (data, end_time - start_time))
	lock.release()

def init(l):
	global lock
	lock = l

if __name__ == '__main__':
	data_list = ['data1', 'data2', 'data3']
	lock = Lock()
	pool = Pool(8, initializer=init, initargs=(lock,))
	pool.map(send_request, data_list)
	pool.close()
	pool.join()
```

这样的修改就没有使用偏函数的必要了。
