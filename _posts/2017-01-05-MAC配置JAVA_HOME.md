---
layout: post
title:  "Mac配置JAVA_HOME"
date:   2017-01-05 15:12:54
categories: 配置环境时候的各种ERROR
tags: JDK
excerpt: JAVA_HOME相关配置。
author: M.renard
mathjax: false
---

* content
{:toc}

首先查看javad 版本

	java -version
	
	java version "1.8.0_111"
	Java(TM) SE Runtime Environment (build 1.8.0_111-b14)
	Java HotSpot(TM) 64-Bit Server VM (build 25.111-b14, mixed mode)
	
查找java的路径

	/usr/libexec/java_home -V
	
	1.8.0_111, x86_64:	"Java SE 8"	/Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home

首先cd到用户目录，然后ls -a查看所有文件，去.bash_profile中配置JAVA_HOME

	vi .bash_profile
	
复制以下代码（根据自己的版本进行修改）

	JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home
	CLASSPAHT=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
	PATH=$JAVA_HOME/bin:$PATH:
	export JAVA_HOME
	export CLASSPATH
	export PATH
	
输入 :wq 保存退出

更新文件：

	source .bash_profile
	
输入

	echo $JAVA_HOME
	
显示刚刚的地址，则代表修改成功