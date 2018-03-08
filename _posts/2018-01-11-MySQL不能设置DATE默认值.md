---
layout: post
title:  "MySQL不能设置DATE默认值"
date:   2018-01-11 15:12:54
categories: 配置环境时候的各种ERROR
tags: MySQL
author: M.renard
mathjax: false
---

* content
{:toc}

由于MySQL目前字段的默认值不支持函数，所以以create_time datetime default now() 的形式设置默认值是不可能的。代替的方案是使用TIMESTAMP类型代替DATETIME类型。

TIMESTAMP列类型自动地用当前的日期和时间标记INSERT或UPDATE的操作。如果有多个TIMESTAMP列，只有第一个自动更新。

自动更新第一个TIMESTAMP列在下列任何条件下发生：

1. 列值没有明确地在一个INSERT或LOAD DATA INFILE语句中指定。
2. 列值没有明确地在一个UPDATE语句中指定且另外一些的列改变值。（注意一个UPDATE设置一个列为它已经有的值，这将不引起TIMESTAMP列被更新，因为如果你设置一个列为它当前的值，MySQL为了效率而忽略更改。）
3. 你明确地设定TIMESTAMP列为NULL.
4. 除第一个以外的TIMESTAMP列也可以设置到当前的日期和时间，只要将列设为NULL，或NOW()。

所以把日期类型 选择成timestamp 允许空就可以了

	CREATE TABLE test (
	uname varchar(50) NOT NULL,
	updatetime timestamp NULL DEFAULTCURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;
	
如果要在navicat下操作的话，将字段设置为timestamp，然后默认值写上CURRENT_TIMESTAMP即可