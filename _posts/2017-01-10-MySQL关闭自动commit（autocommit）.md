---
layout: post
title:  "MySQL关闭自动commit（autocommit）"
date:   2017-01-10 15:12:54
categories: 配置环境时候的各种ERROR
tags: MySQL
author: M.renard
mathjax: false
---

* content
{:toc}

对于mysql来讲，在事务处理时，默认是在动提交的（autocommit），以下方法可以自动关闭autocommit；

案例分析：

1、在mysql登录环境下修改

	[root@mysql2 soft]# mysql -u root -p
	Enter password: 
	Welcome to the MySQL monitor.  Commands end with ; or \g.
	Your MySQL connection id is 4
	Server version: 5.6.25-73.1 Percona Server (GPL), Release 73.1, Revision 07b797f
	Copyright (c) 2009-2015 Percona LLC and/or its affiliates
	Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.
	Oracle is a registered trademark of Oracle Corporation and/or its
	affiliates. Other names may be trademarks of their respective owners.
	Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
	mysql> show databases;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| mysql              |
	| performance_schema |
	| test               |
	+--------------------+
	4 rows in set (0.02 sec)

	mysql> select version();
	+-------------+
	| version()   |
	+-------------+
	| 5.6.25-73.1 |
	+-------------+
	1 row in set (0.00 sec)
	
	mysql> show variables like '%autocommit%';
	+---------------+-------+
	| Variable_name | Value |
	+---------------+-------+
	| autocommit    | ON    |                ；；默认autocommit是开启的
	+---------------+-------+
	1 row in set (0.03 sec)

在当前session关闭autocommit：

	mysql> set @@session.autocommit=0;
	Query OK, 0 rows affected (0.00 sec)

	mysql> show variables like '%autocommit%';
	+---------------+-------+
	| Variable_name | Value |
	+---------------+-------+
	| autocommit    | OFF   |
	+---------------+-------+
	1 row in set (0.00 sec)

在global级别关闭autocommit：

	mysql> set @@global.autocommit=0;
	Query OK, 0 rows affected (0.01 sec)

创建普通用户：

	mysql> create user tom identified by 'tom';
	Query OK, 0 rows affected (0.00 sec)

	mysql> grant all on prod.* to 'tom'@'localhost' identified by 'tom';
	Query OK, 0 rows affected (0.00 sec)

	mysql> flush privileges;
	Query OK, 0 rows affected (0.00 sec)

普通用户登录：

	[root@mysql2 ~]# mysql -u tom -p
	Enter password: 
	Welcome to the MySQL monitor.  Commands end with ; or \g.
	Your MySQL connection id is 6
	Server version: 5.6.25-73.1 Percona Server (GPL), Release 73.1, Revision 07b797f
	Copyright (c) 2009-2015 Percona LLC and/or its affiliates
	Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.
	Oracle is a registered trademark of Oracle Corporation and/or its 
	affiliates. Other names may be trademarks of their respective
	owners.
	Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

	mysql> use mysql;
	ERROR 1044 (42000): Access denied for user 	'tom'@'localhost' to database 'mysql'
	mysql> use  prod;
	Database changed
	mysql> show tables;
	Empty set (0.00 sec)

	mysql> show variables like '%commit%';
	+-------------------------------------------+-------+
	| Variable_name                             | Value |
	+-------------------------------------------+-------+
	| autocommit                                | OFF   |
	| binlog_order_commits                      | ON    |
	| innodb_api_bk_commit_interval             | 5     |
	| innodb_commit_concurrency                 | 0     |
	| innodb_flush_log_at_trx_commit            | 1     |
	| innodb_use_global_flush_log_at_trx_commit | ON    |
	+-------------------------------------------+-------+
	6 rows in set (0.00 sec)


创建测试表：

	mysql> create table t1(id int,name varchar(10));
Query OK, 0 rows affected (0.15 sec)

	mysql> insert into t1 values (10,'tom');
Query OK, 1 row affected (0.00 sec)

	mysql> select * from t1;
	+------+------+
	| id   | name |
	+------+------+
	|   10 | tom
	 |
	+------+------+
	1 row in set (0.00 sec)

事务回滚：

	mysql> rollback;
	Query OK, 0 rows affected (0.02 sec)

	mysql> select * from t1;
	Empty set (0.00 sec)

2、在mysql service重启后

mysql server 重启后：

	[root@mysql2 ~]# service mysql stop
	Shutting down MySQL (Percona Server)....[  OK  ]
	[root@mysql2 ~]# service mysql start
	Starting MySQL (Percona Server).....[  OK  ]
	[root@mysql2 ~]# mysql -u root -p
	Enter password: 
	Welcome to the MySQL monitor.  Commands end with ; or \g.
	Your MySQL connection id is 1
	Server version: 5.6.25-73.1 Percona Server (GPL), Release 73.1, Revision 07b797f
	Copyright (c) 2009-2015 Percona LLC and/or its affiliates
	Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.
	Oracle is a registered trademark of Oracle Corporation and/or its
	affiliates. Other names may be trademarks of their respective
	owners.
	Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

	mysql> show variables like '%commit%';
	+-------------------------------------------+-------+
	| Variable_name                             | Value |
	+-------------------------------------------+-------+
	| autocommit                                | ON    |             ；；autocommit仍然是开启状态
	+-------------------------------------------+-------+
	6 rows in set (0.01 sec)

编辑/etc/my.cnf文件：

	[root@mysql2 ~]# vi /etc/my.cnf
	[mysqld]
	datadir=/var/lib/mysql
	socket=/var/lib/mysql/mysql.sock
	user=mysql
	\# Disabling symbolic-links is recommended to prevent assorted security risks
	symbolic-links=0
	init_connect='set autocommit=0'                                    ；；用户登录时，关闭autocommit
	[mysqld_safe]
	log-error=/var/log/mysqld.log
	pid-file=/var/run/mysqld/mysqld.pid
	explicit_defaults_for_timestamp=true
	innodb_buffer_pool_size = 128M
	join_buffer_size = 128M
	sort_buffer_size = 2M
	read_rnd_buffer_size = 2M

用户登录查看：

	[root@mysql2 ~]# mysql -u root -p
	Enter password: 
	Welcome to the MySQL monitor.  Commands end with ; or \g.
	Your MySQL connection id is 1
	Server version: 5.6.25-73.1 Percona Server (GPL), Release 73.1, Revision 07b797f
	Copyright (c) 2009-2015 Percona LLC and/or its affiliates
	Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.
	Oracle is a registered trademark of Oracle Corporation and/or its
	affiliates. Other names may be trademarks of their respective
	owners.
	Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

	mysql> show variables like '%commit%';
	+-------------------------------------------+-------+
	| Variable_name                             | Value |
	+-------------------------------------------+-------+
	| autocommit                                | ON    |                ；；root用户不受影响（为安全起见）

	mysql> system mysql -u tom -p
	Enter password: 
	Welcome to the MySQL monitor.  Commands end with ; or \g.
	Your MySQL connection id is 2
	Server version: 5.6.25-73.1 Percona Server (GPL), Release 73.1, Revision 07b797f
	Copyright (c) 2009-2015 Percona LLC and/or its affiliates
	Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.
	Oracle is a registered trademark of Oracle Corporation and/or its
	affiliates. Other names may be trademarks of their respective
	owners.
	Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

	mysql> show variables like '%commit%';
	+-------------------------------------------+-------+
	| Variable_name                             | Value |
	+-------------------------------------------+-------+
	| autocommit                                | OFF   |                ；；普通用户，autocommit已被关闭
	+-------------------------------------------+-------+