### postgresql安装

#### 源码安装，在root用户下，执行到 make install后，会将postgres安装在以下的目录下。
```c
/usr/local/pgsql/
```

#### 创造一个postgres的unix用户
```c
adduser postgres
```

#### 再用这个用户，初始化数据库（物理目录，文件）
```c
mkdir /usr/local/pgsql/data
chown postgres /usr/local/pgsql/data
/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data  
```

#### 创建数据库test，并使用它
要注意，使用这个数据库时，psql命令默认会用linux的postgres用户登录。
```c
/usr/local/pgsql/bin/createdb test
/usr/local/pgsql/bin/psql test
```
#### 如果你在别的linux用户下，执行psql test，会报角色错误。例如，你在linux的riddle的用户下使用test库：
```c
riddle@d:~/u$ /usr/local/pgsql/bin/psql test
psql: FATAL:  role "riddle" does not exist
riddle@d:~/u$ 
```

### 安装过程如下：
参考：https://www.postgresql.org/docs/10/static/install-short.html
```
https://ftp.postgresql.org/pub/source/v10.1/postgresql-10.1.tar.bz2
root@d:~/source# tar xvf postgresql-10.1.tar.bz2 
root@d:~/source/postgresql-10.1# apt-get install libreadline-gplv2-dev
root@d:~/source/postgresql-10.1# ./configure
root@d:~/source/postgresql-10.1# make
root@d:~/source/postgresql-10.1# make install
root@d:~/source/postgresql-10.1# adduser postgres
root@d:~/source/postgresql-10.1# mkdir /usr/local/pgsql/data
root@d:~/source/postgresql-10.1# chown postgres /usr/local/pgsql/data
root@d:~/source/postgresql-10.1# su - postgres
postgres@d:~$ cd /usr/local/pgsql/data
postgres@d:/usr/local/pgsql/data$ /usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data  
postgres@d:/usr/local/pgsql/data$ /usr/local/pgsql/bin/postgres -D /usr/local/pgsql/data >logfile 2>&1 &
postgres@d:/usr/local/pgsql/data$ /usr/local/pgsql/bin/createdb test
连接数据库：
postgres@d:/usr/local/pgsql/data$ /usr/local/pgsql/bin/psql test
psql (10.1)
Type "help" for help.

test=# 
test=# 
test=# help
You are using psql, the command-line interface to PostgreSQL.
Type:  \copyright for distribution terms
       \h for help with SQL commands
       \? for help with psql commands
       \g or terminate with semicolon to execute query
       \q to quit
test=# 
``` 

#### 参考：
https://www.postgresql.org/docs/10/static/install-short.html

### libpqxx 安装
```c
root@d:~/source/libpqxx-6.0.0# apt-get install libpq-dev
 wget https://github.com/jtv/libpqxx/archive/6.0.0.tar.gz
 tar xvf 6.0.0........
 cd source/libpqxx-6.0.0
 ./configure
 make
 make install
 ```

