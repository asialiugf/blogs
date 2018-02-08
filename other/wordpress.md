### 
http://blog.csdn.net/qq_31714339/article/details/78237512
```c
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
