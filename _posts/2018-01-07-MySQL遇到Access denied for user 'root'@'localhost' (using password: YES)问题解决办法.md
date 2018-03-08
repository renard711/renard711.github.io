---
layout: post
title:  "MySQL遇到Access denied for user 'root'@'localhost' (using password: YES)问题解决办法"
date:   2018-01-07 15:12:54
categories: 配置环境时候的各种ERROR
tags: MySQL
author: M.renard
mathjax: false
---

* content
{:toc}

首先我们来看三个命令：

* 启动MySQL服务

	sudo /usr/local/mysql/support-files/mysql.server start
	
* 停止MySQL服务

	sudo /usr/local/mysql/support-files/mysql.server stop
	
* 重启MySQL服务

	sudo /usr/local/mysql/support-files/mysql.server restart




第一种解决办法：
重启服务器，有时候在系统设置中Start和Stop按钮会失效，就需要上面的三个命令。

第二种解决办法：
重设密码，具体过程如下：

1. 停止MySQL服务
2. 打开终端输入：sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables
3. 打开另一个新终端，逐行输入：

	sudo /usr/local/mysql/bin/mysql -u root
	
	
	UPDATE mysql.user SET authentication_string=PASSWORD('new password') WHERE 	User='root';
	
	FLUSH PRIVILEGES;
	
	\q
	
4. 重启MySQL
