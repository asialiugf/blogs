### 仅导出一个库的所有表，不含数据
```
./mysqldump -h localhost -uroot -pxxxxxx  -d databasename > /tmp/dump.sql
```

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
```
mysql> use mysql
Database changed
mysql> show tables;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| columns_priv              |
| component                 |
| db                        |
| default_roles             |
| engine_cost               |
| func                      |
| general_log               |
| global_grants             |
| gtid_executed             |
| help_category             |
| help_keyword              |
| help_relation             |
| help_topic                |
| innodb_index_stats        |
| innodb_table_stats        |
| password_history          |
| plugin                    |
| procs_priv                |
| proxies_priv              |
| role_edges                |
| server_cost               |
| servers                   |
| slave_master_info         |
| slave_relay_log_info      |
| slave_worker_info         |
| slow_log                  |
| tables_priv               |
| time_zone                 |
| time_zone_leap_second     |
| time_zone_name            |
| time_zone_transition      |
| time_zone_transition_type |
| user                      |
+---------------------------+
33 rows in set (0.00 sec)

mysql> 
mysql>  update user set authentication_string='111111' where user='root'
    -> ;
Query OK, 0 rows affected (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 0

mysql> 
mysql>  update user set authentication_string='111111' where user='root';
Query OK, 0 rows affected (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 0

mysql> 
```

```
mysql> 
mysql> 
mysql> create user 'wpuser'@'localhost' identified by '123456';               
Query OK, 0 rows affected (0.01 sec)

mysql> 
mysql> 
mysql> grant all privileges on *.* to 'wpuser'@'localhost';
Query OK, 0 rows affected (0.00 sec)

mysql>flush privileges;
```

```
#查看权限
show grants for '用户'@'IP地址'

#授权 mjj用户仅对db1.t1文件有查询、插入和更新的操作
grant select ,insert,update on db1.t1 to "alex"@'%';

# 表示有所有的权限，除了grant这个命令，这个命令是root才有的。mjj用户对db1下的t1文件有任意操作
grant all privileges  on db1.t1 to "alex"@'%';
#mjj用户对db1数据库中的文件执行任何操作
grant all privileges  on db1.* to "alex"@'%';
#mjj用户对所有数据库中文件有任何操作
grant all privileges  on *.*  to "alex"@'%';
 
#取消权限
 
# 取消mjj用户对db1的t1文件的任意操作
revoke all on db1.t1 from 'alex'@"%";  

# 取消来自远程服务器的mjj用户对数据库db1的所有表的所有权限

revoke all on db1.* from 'alex'@"%";  

取消来自远程服务器的mjj用户所有数据库的所有的表的权限
revoke all privileges on *.* from 'alex'@'%';

MySql备份命令行操作
```
