---
layout: post
title:  "Oracle SQL Developer MAC版修改为中文界面的方法"
date:   2018-01-09 15:12:54
categories: 配置环境时候的各种ERROR
tags: SQL
author: M.renard
mathjax: false
---

* content
{:toc}

虽非必要，不过能改过来看着也挺顺眼的……方法如下：

打开：/Applications/SQLDeveloper 2.app/Contents/Resources/sqldeveloper/ide/bin/ide.conf 加入如下两行配置文件：

```
AddVMOption -Duser.language=zh       
AddVMOption -Duser.country=CN
```
