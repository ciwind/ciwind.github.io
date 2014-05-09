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

* mysql缓存设置，编辑`/var/my.cnf`

添加以下配置：
<pre class="code prettyprint linenums">
query_cache_size = 268435456
query_cache_type = 1
query_cache_limit = 1048576
</pre>

查看cache占用情况：
<pre class="code prettyprint linenums">
show status like 'Qca%';
show status like 'Com_sel%';
</pre>

* 获取表大小、行数

<pre class="code prettyprint linenums">
SELECT 
    table_schema,
    table_name, 
    table_rows, 
    ROUND((data_length+index_length)/1024/1024) AS total_mb,  
    ROUND(data_length/1024/1024) AS data_mb,  
    ROUND(index_length/1024/1024) AS index_mb  
FROM INFORMATION_SCHEMA.TABLES 
WHERE TABLE_SCHEMA='testdb';
</pre>

* 添加用户、授权

<pre class="code prettyprint linenums">
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypass';
grant select on dbname.tbname to username@localhost;
flush privileges;
</pre>

* 批量改变存储引擎

<pre class="code prettyprint linenums">
select 
    concat('alter table ', table_name, ' ENGINE=InnoDB') 
from information_schema.tables 
where table_schema="db_test" and ENGINE="MyISAM";
</pre>






