---
title: Linux下Git Server搭建
tags: [技术, 运维, Linux]
categories: [技术, 运维]
date: 2016-09-07 05:38:21
updated: 2018-06-02 04:26:08
---

以Ubuntu系统为例讲解如何搭建一个git服务器

简单的说步骤如下：

* 安装相关软件
* 创建用户
* 创建证书登录
* 创建仓库

<!-- more -->

有些时候由于各种原因我们不能使用公共的git服务，所以我们需要自己搭建一个GIT服务器以方便项目合作。

搭建GIT服务器首先需要有一个Linux服务器，这篇文章以Ubuntu环境为例简单讲解该如何搭建一个GIT服务器。

搭建一个GIT服务器大概有这么几个步骤：安装相关软件，创建用户，创建证书登录，创建仓库。

下面详细说说每一步该怎么做：

首先需要安装必须的软件

```bash
sudo apt-get install git
sudo apt-get install openssh-server
```

接下来就是为git服务创建一个专用的账户

```bash
sudo adduser git
```

对于用户较少的团队大家可以直接用git账户来拖代码或者推代码，但是这样的做法是不安全的，同时也很难区分作者。

最好的办法可以是让大家都上传自己的公钥，由管理员添加进允许列表，具体是这样做的。

首先需要修改ssh配置，在/etc/ssh/目录下有一个sshd_config的文件。

找到里面有关AuthorizedKeysFile的一行，取消注释，并将后面的路径修改为/home/git/.ssh/authorized_keys

保存后重启ssh服务。

接下来就是可以处理用户登录密钥的问题了。

对于熟悉git shell的用户可以直接使用

```bash
ssh-keygen -t rsa
```

命令来生成密钥，然后将生成的.pub文件给管理员即可。对于不熟悉的用户，可以使用带有图形化界面的git软件source tree。在工具中有创建或导入ssh密钥。点击Generate，然后在空白区域移动鼠标即可。待生成完毕之后可以把Key comment里的内容改为自己的名字。将上方框里的Public key给管理员即可。

管理员拿到所有的密钥文件之后将这些密钥文件每个一行添加到之前说的authorized_keys文件中即可。

添加完成后重启。

接下来管理员就可以创建仓库了。

比如/var/git/目录作为git仓库的存储目录，则可以在这里创建空的git项目，比如我们可以通过下面的命令来完成：

```bash
sudo git init --bare test.git
```

这样GIT就创建了一个裸仓库，接下来我们把这个仓库的owner改为git

```bash
sudo chown -R git:git test.git
```

这样的话客户端已经可以正常通过clone命令来克隆仓库了。

```bash
git clone git@192.168.1.xxx:/var/git/test.git
```

然后就可以正常推送和使用了。如果在克隆的时候需要输入密码，那就是因为授权验证的ssh-agent没有将密钥随着请求，可以使用ssh-add命令来添加。

为了方便使用，建议客户端配置git config --global，这样的话git server端就可以很清楚的知道每次提交都是谁提交的了。

```bash
git config --global user.name yumendy
git config --global user.email yumendy@163.com
```

这样的话在push代码的时候就可以不用在设置这些信息了。

接下来需要解决的是一个安全性问题，就是需要禁止git用户的shell登录。

我们只需要将/etc/passwd文件里的

```
git:x:1001:1001:,,,:/home/git:/bin/bash
```

后的/bin/bash改为gitshell的路径就可以了。一般是/usr/bin/git-shell

即修改为：

```html
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

重启。

这样的话一个简易的git版本控制服务端就搭建好了，客户端可以通过图形化界面的source tree完成代码的推送拉取等任务。
