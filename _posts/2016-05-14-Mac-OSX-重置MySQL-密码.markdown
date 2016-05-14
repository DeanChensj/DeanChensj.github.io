---
layout: post
title:  "Mac OSX 重置 MySQL 密码"
date:   2016-05-14 10:30:00 +0800
categories: Technical
---

**首先声明我是web基础约等于零的渣渣。**
		
今天遇到一个问题，OSX下MySQL密码忘记了...大写的尴尬 : )

<code> <br>
	Dean$ mysql -u root -p<br>
	Enter password:
	ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
</code>

####解决办法：
首先进入系统Preference，选择MySQL, 点击 ‘Stop MySQL Server’.

打开terminal， 进入mysql启动目录并开启root权限,消除MySQL的安全检查.

<code><br>Dean$ cd /usr/local/mysql/bin/
<br>Dean-Mac:bin Dean$ sudo su <br>
sh-3.2# ./mysqld_safe --skip-grant-tables & <br>

</code>

进入mysql，要做的就是重置密码.
<code><br>
sh-3.2# ./mysql <br>
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8 <br>
Server version: 5.7.9 MySQL Community Server (GPL) <br><br>
Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved. <br><br>
Oracle is a registered trademark of Oracle Corporation and/or its affiliates. Other names may be trademarks of their respective owners. <br><br>
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement. <br><br>
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec) <br><br>
mysql> update mysql.user set authentication_string=password('123456') where user='root';
Query OK, 0 rows affected, 1 warning (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 1 <br> <br>
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec) <br> <br>
mysql> exit;
Bye
</code> 






