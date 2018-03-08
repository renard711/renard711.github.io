---
layout: post
title:  "Eclipse 遇到 mac A Java Runtime Environment (JRE) or Java Development Kit (JDK) must be XXXX 问题解决办法"
date:   2018-01-01 15:12:54
categories: 配置环境时候的各种ERROR
tags: Eclipse JDK
author: M.renard
mathjax: false
---

* content
{:toc}

一般是删除了旧的JDK，安装新的JDK以后遇到的打不开的问题。

Linux下：终端进入eclipse目录，然后输入：
mkdir jre
cd jre
ln -s 你的JDK目录/bin bin(注意：ln 是 小写的 L ，不是 i )
上述意思就是在eclipse目录下创建一个jre目录，然后在里面创建jdk/bin目录的快捷方式

Mac下：打开eclipse包内容，然后Eclipse → eclipse.ini

	-vm
	/Library/Java/JavaVirtualMachines/jdk1.8.0_152.jdk/Contents/Home/jre/bin
	
修改这里的JDK地址~解决问题~




