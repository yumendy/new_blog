---
title: Ubuntu更新时提示/boot空间不足解决方案
date: 2014-10-13 08:55:00
updated: 2018-05-29 14:21:22
tags: [技术, 运维, Linux]
categories: [技术, 运维]
---

*卸载Linux旧版内核相关*

<!-- more -->

由于linux每次更新新的内核时不会卸载旧的内核，所以boot的空间会被占用很多，导致新的更新不能正常安装。这时候就需要用户手动清理旧的内核来腾出空间供新的更新使用。

查看已经安装的内核：

```bash
dpkg --get-selections |grep linux-image
```

查看当前启动的内核（理论上讲，除了这个之外其他的都可以卸载了，不过建议保留一两个之前版本）：

```bash
uname -a
```

卸载老版本的内核：

```bash
sudo apt-get purge linux-image-3.13.0-34-generic # 卸载linux-image-3.13.0-34-generic内核
```

ok！问题解决了。
