### 
http://blog.csdn.net/qq_31714339/article/details/78237512
```c

root@iZ23psatkqsZ:/var/www/html# chown -R www-data:www-data *

service php7.0-fpm restart 
service apache2 restart
service mysql restart

vim /etc/apache2/apache2.conf
 vi /etc/apache2/sites-enabled/000-default.conf
```
### mysql数据库操作
```
rabbit@iZ23psatkqsZ:~$ mysql -u root -p 
Enter password: 
```

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| wordpressdb        |
+--------------------+
5 rows in set (0.03 sec)

mysql> 
```
```
mysql> CREATE DATABASE wordpressdb;
mysql> GRANT ALL PRIVILEGES ON wordpressdb.* TO wpuser@localhost;
mysql> FLUSH PRIVILEGES;
mysql> select user from mysql.user;  
+------------------+
| user             |
+------------------+
| debian-sys-maint |
| mysql.session    |
| mysql.sys        |
| root             |
| wpuser           |
+------------------+
5 rows in set (0.00 sec)

mysql> 
```
```
mysql> SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
mysql> use  mysql;
mysql> show tables;
```
### 备份数据库
```
rabbit@iZ23psatkqsZ:~$ mysqldump -uroot -p wordpressdb>kkk.sql

rabbit@iZ23psatkqsZ:~$ mysql -u root -p 
mysql> CREATE DATABASE wpdb;
mysql> GRANT ALL PRIVILEGES ON wpdb.* TO wpuser@localhost;        
mysql> FLUSH PRIVILEGES;
mysql> use wpdb;
mysql> show tables;
Empty set (0.00 sec)
mysql> quit
Bye
```
### 恢复数据库
```
rabbit@iZ23psatkqsZ:~$ mysql -uroot -p wpdb < kkk.sql   
```
### 以后可能用到的插件

WP Editor.md被成功删除。

WordPress支付宝Alipay|财付通Tenpay|贝宝PayPal集成插件
启用 | 删除
WordPress支付宝Alipay|财付通Tenpay|贝宝PayPal集成插件, 集成支付宝,财付通,贝宝,网银,V3:支持支付宝多接口!

3.7.2版本 | 由歪SIR | 查看详情


