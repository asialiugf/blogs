### 【mysql 安装在普通用户下】

```
su - maintain
tar xvf /tmp/mysql-5.7.23-el7-x86_64.tar.gz
mv mysql-5.7.23-el7-x86_64 mysql-5.7.23
mkdir /home/maintain/mysql

```
```
cd /home/maintain/mysql
cat my.cnf
```
#### /home/maintain/mysql/my.cnf
```
maintain@develop:~/mysql$ cat my.cnf
[client]
#password    = your_password
port        = 8302
socket        = /home/maintain/mysql/mysql8302.sock

# Here follows entries for some specific programs

# The MySQL server
[mysqld]
user=root
bind-address = 0.0.0.0
port        = 8302
socket           = /home/maintain/mysql/mysql8302.sock
pid-file         = /home/maintain/mysql/mysql.pid
basedir          = /home/maintain/mysql-5.7.23
datadir          = /home/maintain/mysql/data
tmpdir           = /home/maintain/mysql/tmp
log-error        = /home/maintain/mysql/log/mysql.err
general_log_file = /home/maintain/mysql/log/mysql.log
general_log      = 1
skip-grant-tables
maintain@develop:~/mysql$ 
```
#### 初始化数据库

* 注意， 要加 --user=mysql，不然的话， 不会创建 mysql库，不能执行这一句 mysql> use mysql;
```
maintain@develop:~/mysql-5.7.23/bin$ ./mysqld --defaults-file=/home/maintain/mysql/my.cnf --initialize
maintain@develop:~/mysql-5.7.23/bin$ ./mysqld --defaults-file=/home/maintain/mysql/my.cnf --initialize --user=mysql
maintain@develop:~/mysql-5.7.23/bin$ ./mysqld --defaults-file=/home/maintain/mysql/my.cnf &
```
#### 开启 
```
maintain@develop:~/mysql-5.7.23/bin$ ./mysqld --defaults-file=/home/maintain/mysql/my.cnf &
```

#### 连接数据库
```
maintain@develop:~/mysql-5.7.23/bin$ ./mysql --socket=/home/maintain/mysql/mysql8302.sock
```
#### 改密码

```
maintain@develop:~/mysql$ cd ../mysql-5.7.23/bin
maintain@develop:~/mysql-5.7.23/bin$ ./mysql --socket=/home/maintain/mysql/mysql8302.sock
mysql> show databases;
mysql> use mysql;
mysql>  update user set authentication_string='111111' where user='root';
```

```
maintain@develop:~/mysql$ cd ../mysql-5.7.23/bin
maintain@develop:~/mysql-5.7.23/bin$ ./mysql --socket=/home/maintain/mysql/mysql8302.sock
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.23-log MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> 
mysql> 
mysql> 
mysql>  update user set authentication_string='111111' where user='root';
ERROR 1046 (3D000): No database selected
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql>  update user set authentication_string='111111' where user='root';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> 
```


***
***
