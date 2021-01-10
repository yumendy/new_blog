---
title: 使用nginx+uwsgi部署django应用
tags: [技术, 运维, Python, Django]
categories: [技术, 运维]
date: 2016-08-25 04:48:49
updated: 2018-06-01 00:03:00
---

简单明了的说明如何在Linux下部署一个Django应用

* 这篇文章好像拖了很久的样纸w
* 好吧，其实可以把分割线里面的东西打印下来当手册，每次部署的时候查一下。

<!-- more -->

唔~很早就答应修修写一篇关于django部署的文档了，但是由于最近成都的天气实在是太热，完全处于挺尸状态什么也不想做，所以就耽误了下来（喂喂喂，段喵喵你确定不是因为沉迷于基三不能自拔嘛）。咳咳。。。

好了，不说废话了，还是直接说说正经的吧。

心急的人只看分割线内部的东东就足够了。不着急的请跳过分割线。

先明确一个概念，文章中所有说的django项目目录，指的是manage.py所在的目录。

文章里的项目名称指的是你在创建这个django app的时候起的名字。

在下面的例子中我都假装我创建了一个叫DEMO的项目，大家按需把DEMO换为自己的项目名称。

假装这个项目我是要部署在yumendy.com这个域名下面。

----------我是萌萌的分割线----------

这里的例子是以Ubuntu下部署为例的。假装你现在拿到的是一个全新的Ubuntu

1. 安装必须的软件

```bash
sudo apt-get install python-pip
sudo apt-get install nginx
sudo apt-get install python-dev

pip install django
pip install pillow
pip install uwsgi
```

2. 修改django配置

修改settings.py文件。

加入如下四行代码：

```python
STATIC_URL = '/static/'
MEDIA_URL = '/media/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static').replace('\\\\', '/')
MEDIA_ROOT = os.path.join(BASE_DIR, 'media').replace('\\\\', '/')
```

在生成环境下可以把settings.py里的DEBUG改为False，别忘了在ALLOWED_HOSTS里加一个'*'，即改为

```python
DEBUG = False
ALLOWED_HOSTS = ['*']
```

然后在项目目录里创建两个文件：

`DEMO.conf`

里面写如下内容

```
server {
    listen   80;
    
    server_name www.yumendy.com yumendy.com;
    access_log /var/log/nginx/yumendy.com.log ;
    error_log /var/log/nginx/yumendy.com.log ;
    
    location / {
            uwsgi_pass 127.0.0.1:8800;
            include uwsgi_params;
    }
    
    location ~/static/ {
            root  /var/www/DEMO/;
            index  index.html index.htm;
    }
    
    location ~/media/ {
            root  /var/www/DEMO/;
            index  index.html index.htm;
    }
}
```

这是一个nginx的配置文件，解释一下上面的内容：

第2行的80是端口号，可以按需修改。

第4行的server_name后面写自己的域名，没有的话可以写localhost或者127.0.0.1

第5、6两行是日志路径可以自己修改。

第9行是与uwsgi通信的地址和端口，这里也可以使用socket文件进行通信，但是建议使用端口更容易配置一些。

第14行是将来需要存放静态文件文件夹的路径，我的静态文件的文件夹是放在项目目录下的，项目目录随后会放在/var/www/目录下所以我这么写了。

第19行同理，只不过是media文件的配置。

再创建一个文件：

`DEMO.xml`

里面是如下内容

```xml
<uwsgi>
    <socket>127.0.0.1:8800</socket>
    <chdir>/var/www/DEMO</chdir>
    <module>DEMO.wsgi:application</module>
    <processes>1</processes>
    <daemonize>/var/log/uwsgi.log</daemonize>
</uwsgi>
```

这是一个uwsgi运行的配置文件，解释一下内容：

第2行是说uwsgi监听的端口，注意此处我写的是8800这个需要与之前的conf文件里的第9行保持统一，如果在一个server上部署多个django项目，请注意区别一下这个端口。

第3行是项目路径

第4行是wsgi应用入口，你只需要将前面的DEMO改为你的项目名称就好

第5行是uwsgi进程数，建议与逻辑处理器保持一致

第6行是日志路径可以自己配置

3. 打包上传

把项目上传到服务器上并且解压缩，我是丢到了/var/www/目录下

4. 部署

进入/etc/nginx/site-enable/目录下执行如下命令

```bash
sudo ln -s /var/www/DEMO/DEMO.conf
sudo service nginx restart
```

进入/var/www/DEMO/目录，执行如下命令

```bash
sudo python manage.py collectstatic
sudo uwsgi -x DEMO.xml
```

5. 加入开机自启

修改/etc/rc.local文件，在exit 0这一行之上加入

```bash
sudo uwsgi -x /var/www/DEMO.xml
```

完

----------我是萌萌的分割线----------

嘿~其实说不着急的人看这里就是想简单解释一下分割线里面的东西是什么，以及我们为什么要这么做。

一般情况下，一个客户端发起一个http的requset请求到服务端返回一个response就是一个完整的流程。

因为服务端的各种语言、各种框架种类繁多，所以我们必须有一个标准来使一些工作变得可以复用。

在这里我们使用nginx作为http server，它主要有两个作用，第一个是作为反向代理，可以将来自不同域名的请求委托给不同的后台服务来处理，这样就可以在一个server上部署多个项目了，语言不同也完全没有关系。第二个重要的工作就是管理静态文件。可能有些童鞋会问了，为什么要用nginx来管理静态文件的请求呢，交给Python不是一样可以吗？这个问题其实很容易回答，大家不要忘记效率问题，django处理静态文件的方法非常简单粗暴，用open命令打开文件，然后读取，再封装成request返回。对于大量细小的文件访问操作，效率可想而知，所以交给nginx是一个不错的选择，现代的浏览器都是可以同时发起多个请求的，在多进程的处理上，nginx也优于直接使用django。

接下来想简单介绍一下uWSGI，uWSGI是一个web服务器软件，它实现了两种协议，一种是uwsgi还有一种是WSGI，这里的WSGI是Python web服务网关接口。是一种协议，简而言之是一种约定，所有的框架只要按这个协议实现自己的WSGI接口的web应用，就可以很方便的和支持这个协议的web server对接了。在这里我们选择的正是uwsgi这个web server。知道了这一点的话。其实大家学会了django的部署也就会了其他python web应用的部署。

那么现在整个流程就比较清楚了，客户端发起一个请求，被nginx接收，如果请求是静态文件（其实就是判断访问的url是不是static或者media）则直接由nginx返回静态文件，如果不是静态文件，则委托给uwsgi来处理，uwsgi根据配置文件来调用相对应的WSGI接口的web应用。取回响应内容再交还给nginx返回用户。

这个流程有什么好处呢？

其实主要是Nginx比起直接使用uwsgi来，作为一个专业的http server其安全性更好，而且在进程分配和维持用户连接上有更多的优势。

那么看完了这些可以直接上去看分割线里面的东西了。一下子会变得好理解很多呢w
