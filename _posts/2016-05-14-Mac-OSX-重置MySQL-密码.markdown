---
layout: post
title:  "Mac OSX 重置 MySQL 密码"
date:   2016-05-14 10:30:00 +0800
categories: Technical
---

**首先声明我是web基础约等于零的渣渣。**
		
今天遇到一个问题，Mac OSX下MySQL密码忘记了...大写的尴尬 : )


	Dean$ mysql -u root -p<br>
	Enter password:
	ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)


####解决办法：
首先进入系统Preference，选择MySQL, 点击 ‘Stop MySQL Server’.

打开terminal， 进入mysql启动目录并开启root权限,消除MySQL的安全检查.

	Dean$ cd /usr/local/mysql/bin/
	<br>Dean-Mac:bin Dean$ sudo su <br>
	sh-3.2# ./mysqld_safe --skip-grant-tables &
	[1] 17998
	sh-3.2# 160514 11:49:05 mysqld_safe Logging to '/usr/local/mysql/data/
	Dean-Mac.local.err'.160514 11:49:05 mysqld_safe Starting mysqld daemon with databases from /usr/local/mysql/data

进入mysql，要做的就是重置密码.

<pre>
sh-3.2#  ./mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 5.7.9 MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
</pre>

在更改密码之前和之后更新系统权限表, 将'YOURNEWPASS' 引号中的部分替换为你的新密码即可.
<pre>
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec) 

mysql> update mysql.user set authentication_string=password('YOURNEWPASS') where user='root';

Query OK, 0 rows affected, 1 warning (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 1 

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec) 

mysql> exit;
Bye
</pre>

再次召唤MySQL即可.

	Dean$ mysql -uroot -pYOURNEWPASS






