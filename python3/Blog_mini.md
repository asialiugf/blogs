* http://blog.51cto.com/xpleaf/1748871
* http://blog.51cto.com/xpleaf/1748629
* https://github.com/xpleaf/Blog_mini

```python
(venv) [gethtml@iZ23psatkqsZ ~/Blog_mini]$ deactivate 
[gethtml@iZ23psatkqsZ ~/Blog_mini]$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 8.0.12 MySQL Community Server - GPL

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database blog_mini default character set utf8 collate utf8_general_ci;
Query OK, 1 row affected, 1 warning (0.36 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| blog_mini          |
| djangodb           |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
7 rows in set (0.01 sec)

mysql> 
```
