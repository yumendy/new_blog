---
title: 将Python脚本打包为exe文件
tags: [技术, 开发, Python]
categories: [技术, 开发]
date: 2015-02-10 15:49:00
updated: 2018-05-30 10:30:09
---

*简单几步将python脚本打包为exe包*

第一步：在https://pypi.python.org/pypi/PyInstaller/2.1 下载pyinstaller。

第二步：解压缩，在该目录下命令行中执行python setup.py install。

第三步：在需要编译的文件目录中在命令行中执行： pyinstaller filename.py即可。

第四步：在dist文件夹中找到exe文件即可。

注意：务必关闭杀毒软件和卫士等等~否则会报毒。
