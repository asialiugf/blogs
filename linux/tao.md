### 查看centos版本
```c
[root@localhost etc]# cat /etc/issue
CentOS release 6.8 (Final)
Kernel \r on an \m

[root@localhost etc]#
```
```c
[root@localhost etc]# uname -a
Linux localhost.localdomain 2.6.32-642.11.1.el6.x86_64 #1 SMP Fri Nov 18 19:25:05 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
[root@localhost etc]# 
```
```c
[root@localhost etc]# cat /proc/version
Linux version 2.6.32-642.11.1.el6.x86_64 (mockbuild@c1bm.rdu2.centos.org) (gcc version 4.4.7 20120313 (Red Hat 4.4.7-17) (GCC) ) #1 SMP Fri Nov 18 19:25:05 UTC 2016
[root@localhost etc]#
```

### 查看mysql版本
```c
[root@localhost /]# find . -name mysql
./var/mysql
./var/mysql/include/mysql
./var/mysql/data/mysql
./var/mysql/bin/mysql

[root@localhost bin]# ./mysql --help | grep Distrib 
./mysql  Ver 14.14 Distrib 5.5.20, for linux2.6 (x86_64) using readline 5.1
[root@localhost bin]# 
```

```c
[root@localhost bin]# ./mysql -uroot -p5x9t3c2m
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 28483449
Server version: 5.5.20-log MySQL Community Server (GPL)

Copyright (c) 2000, 2011, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```
```c
mysql>  select version(); 
+------------+
| version()  |
+------------+
| 5.5.20-log |
+------------+
1 row in set (0.00 sec)

mysql> 
```
```c
mysql>  status; 
--------------
./mysql  Ver 14.14 Distrib 5.5.20, for linux2.6 (x86_64) using readline 5.1

Connection id:          28483449
Current database:
Current user:           root@localhost
SSL:                    Not in use
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.5.20-log MySQL Community Server (GPL)
Protocol version:       10
Connection:             Localhost via UNIX socket
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
UNIX socket:            /tmp/mysql.sock
Uptime:                 431 days 19 hours 44 min 39 sec

Threads: 1  Questions: 947504446  Slow queries: 107991  Opens: 3813413  Flush tables: 1  Open tables: 256  Queries per second avg: 25.395
--------------

mysql> 
```
```c
[root@localhost bin]# rpm -qa|grep mysql 
qt-mysql-4.6.2-28.el6_5.x86_64
mysql-libs-5.1.73-7.el6.x86_64
[root@localhost bin]# 
```

### mysql常用命令
```c
mysql> show databases;
```
### PHP版本
```c
[root@localhost bin]# ./php -v
PHP 5.4.45 (cli) (built: Sep 10 2016 03:22:56) 
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2014 Zend Technologies
[root@localhost bin]# 
```



