### 改root密码
* 在 my.cnf文件中增加 skip-grant-tables 
```
[root@iZ23psatkqsZ ~]# vim /etc/my.cnf 

datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
skip-grant-tables 
```
```
service mysqld stop
service mysqld start
```
然后再执行 mysql命令，修改密码

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
5 rows in set (0.16 sec)

mysql>
```
