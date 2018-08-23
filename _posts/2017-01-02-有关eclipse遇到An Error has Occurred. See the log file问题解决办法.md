---
layout: post
title:  "有关eclipse遇到An Error has Occurred. See the log file问题解决办法"
date:   2017-01-02 15:12:54
categories: 配置环境时候的各种ERROR
tags: Eclipse
author: M.renard
mathjax: false
---

* content
{:toc}

方法1：删除eclipse\configuration 目录下的 org.eclipse.osgi org.eclipse.update 两个子目录，重新启动 eclipse；

方法2：删除workspace中.metadata；

方法3：打开eclipse包内容，打开 \configuration\.settings\org.eclipse.ui.ide.prefs文件，修改其中的“SHOW_WORKSPACE_SELECTION_DIALOG”为true；

以上办法基本能解决一切问题，然而我的依然不行，于是打开workspace中的.log文件，里面显示我的java -version 是jdk9。查看我的path配置，确实是1.8。应该是昨天装了一次jdk9以后修改过path文件然后出现了不知道哪里的问题。怒卸载jdk9：

	sudo rm -rf /Library/Java/JavaVirtualMachines/jdk9.jdk
	
这回弹出的问题就改为了JDK的问题了。卸载了eclipse重装，一切都好了起来。