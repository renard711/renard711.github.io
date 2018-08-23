---
layout: post
title:  "IDEA 遇到 Class JavaLaunchHelper is implemented in both XXX 问题解决办法"
date:   2017-01-04 15:12:54
categories: 配置环境时候的各种ERROR
tags: IDEA
author: M.renard
mathjax: false
---

* content
{:toc}

方法1：Help → Edit Custom Properties → 添加：

	idea.no.launcher=true
		
方法2：这个算是Mac的Java老bug了，更新JDK9或者Java8u152可以解决

删除之前jdk：

	sudo rm -rf /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk
	
绝对解决问题~