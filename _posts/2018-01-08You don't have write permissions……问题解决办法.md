---
layout: post
title:  "You don't have write permissions……问题解决办法"
date:   2018-01-08 15:12:54
categories: 配置环境时候的各种ERROR
tags: Ruby OSX/Linux
author: M.renard
mathjax: false
---

* content
{:toc}

安装Ruby的时候遇到了权限问题，本来以为很简单，以往的方法都是：进入`usr/bin/`文件夹，修改文件信息，改为可读写即可，但是却并不能执行，提示“不能完成此操作，您没有必要的权限”。

后来查阅了一下，原来是OS X 10.11以后的新安全特性……

OS X 10.11 加入了Rootless，默认创建的用户还是属于admin用户组，也能切换到root用户，但是加以了限制，结果是一旦你执行su 或者 sudo切换到root用户。但是你的这个root用户不再是真正的root用户(对所有文件有rwx)！

当然，问题还是能解决的：

**重启电脑，按住 command + R，出现 OS X Utilities 界面后，在 Utilities 菜单中选择 Terminal ，运行 “csrutil disable; reboot” ，电脑自动重启。**

**（开启 sip “csrutil enanbel；reboot”）**
