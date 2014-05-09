---
layout: post
title: "mysql, skills"
description: "mysql使用技巧"
category: skills
tags: [mysql]
---
{% include JB/setup %}

####记录mysql使用过程中碰到的问题及解决方案。

* mysql启动

`/etc/init.d/mysql start`，若启动失败，通过`service mysql status`查看状态：

错误：`mysql is not running but lock exists`，可以尝试 `rm /var/lock/subsys/mysql`

错误：`MySQL is running but PID file could not be found`，在无需做备份、回滚的情况下，找到mysql进程`ps aux | grep mysqld`，`kill -9 进程号`后重试

若还是无法启动，查看mysql数据目录下是否有`mysql-bin.*`文件，若无需备份、回滚，删除之，重试之



