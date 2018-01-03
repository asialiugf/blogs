### 使用postgresql

#### 初始化数据库文件
这里，在一个新的linux用户下初始化数据库文件
```c
su - dbrun
mkdir data
cd ~/data
pwd
/home/dbrun/data
/usr/local/pgsql/bin/initdb -D .
```
#### 在linux的dbrun用户下执行下面命令，启动数据库进程
修改~/data/postgresql.conf这个文件的默认 port = 6688
```c
/usr/local/pgsql/bin/pg_ctl -D /home/dbrun/data -l logfile start
```

#### 创建数据库并使用，库名叫future
```
/usr/local/pgsql/bin/createdb -p 6688 future
/usr/local/pgsql/bin/createdb -p 6688 future
```

```c
dbrun@d:~$ /usr/local/pgsql/bin/createdb -p 6688 future
dbrun@d:~$ 
dbrun@d:~$ 
dbrun@d:~$ /usr/local/pgsql/bin/psql -p 6688
psql: FATAL:  database "dbrun" does not exist
dbrun@d:~$ 
dbrun@d:~$ 
dbrun@d:~$ /usr/local/pgsql/bin/psql -p 6688 future
psql (10.1)
Type "help" for help.

future=# 
future=# 
```
##### 创建完了后，只是dbrun用户可以使用，其它linux用户，比如riddle上不来
```
riddle@d:~$ /usr/local/pgsql/bin/psql -p 6688 future
psql: FATAL:  role "riddle" does not exist
riddle@d:~$ 
```

### 【基本使用】

#### 查看有多少个数据库 
```
future=# \l
                              List of databases
   Name    | Owner | Encoding |   Collate   |    Ctype    | Access privileges 
-----------+-------+----------+-------------+-------------+-------------------
 future    | dbrun | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres  | dbrun | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | dbrun | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/dbrun         +
           |       |          |             |             | dbrun=CTc/dbrun
 template1 | dbrun | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/dbrun         +
           |       |          |             |             | dbrun=CTc/dbrun
(4 rows)

future=# 
```
#### 查看一个数据库的大小
```
postgres=# 
postgres=# select pg_database_size('future'); 
 pg_database_size 
------------------
          7509479
(1 row)

postgres=# select pg_database_size('postgres');
 pg_database_size 
------------------
          7509479
(1 row)

postgres=# 
```
#### 查看所有数据库的大小
```
postgres=# 
postgres=#  select pg_database.datname, pg_database_size(pg_database.datname) AS size from pg_database; 
  datname  |  size   
-----------+---------
 postgres  | 7509479
 future    | 7509479
 template1 | 7373315
 template0 | 7373315
(4 rows)

postgres=# 
```

#### 查看表空间的大小
```
future=# 
future=# select spcname from pg_tablespace; 
  spcname   
------------
 pg_default
 pg_global
(2 rows)

future=# 
future=# select pg_size_pretty(pg_tablespace_size('pg_default'));
 pg_size_pretty 
----------------
 28 MB
(1 row)

future=#
```


#### 【创建数据库详细的内容如下：】
```c
root@d:~# 
root@d:~# adduser dbrun
Adding user `dbrun' ...
Adding new group `dbrun' (1007) ...
Adding new user `dbrun' (1007) with group `dbrun' ...
Creating home directory `/home/dbrun' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for dbrun
Enter the new value, or press ENTER for the default
        Full Name []: 
        Room Number []: 
        Work Phone []: 
        Home Phone []: 
        Other []: 
Is the information correct? [Y/n] 
root@d:~# 
root@d:~# 
root@d:~# 
root@d:~# su - dbrun
dbrun@d:~$ ll
total 20
drwxr-xr-x  2 dbrun dbrun 4096 Jan  3 15:18 ./
drwxr-xr-x 10 root  root  4096 Jan  3 15:18 ../
-rw-r--r--  1 dbrun dbrun  220 Jan  3 15:18 .bash_logout
-rw-r--r--  1 dbrun dbrun 3771 Jan  3 15:18 .bashrc
-rw-r--r--  1 dbrun dbrun  655 Jan  3 15:18 .profile
dbrun@d:~$ 
dbrun@d:~$ 
dbrun@d:~$ 
dbrun@d:~$ pwd
/home/dbrun
dbrun@d:~$ 
dbrun@d:~$ mkdir data
dbrun@d:~$ ll
total 24
drwxr-xr-x  3 dbrun dbrun 4096 Jan  3 15:18 ./
drwxr-xr-x 10 root  root  4096 Jan  3 15:18 ../
-rw-r--r--  1 dbrun dbrun  220 Jan  3 15:18 .bash_logout
-rw-r--r--  1 dbrun dbrun 3771 Jan  3 15:18 .bashrc
drwxrwxr-x  2 dbrun dbrun 4096 Jan  3 15:18 data/
-rw-r--r--  1 dbrun dbrun  655 Jan  3 15:18 .profile
dbrun@d:~$ pwd
/home/dbrun
dbrun@d:~$ cd data
dbrun@d:~/data$ ll
total 8
drwxrwxr-x 2 dbrun dbrun 4096 Jan  3 15:18 ./
drwxr-xr-x 3 dbrun dbrun 4096 Jan  3 15:18 ../
dbrun@d:~/data$ pwd
/home/dbrun/data
dbrun@d:~/data$ 
dbrun@d:~/data$ 
dbrun@d:~/data$ 
dbrun@d:~/data$ /usr/local/pgsql/bin/initdb -D .
The files belonging to this database system will be owned by user "dbrun".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.UTF-8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory . ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    /usr/local/pgsql/bin/pg_ctl -D . -l logfile start

dbrun@d:~/data$ 
dbrun@d:~/data$ 
dbrun@d:~/data$ ll
total 120
drwx------ 19 dbrun dbrun  4096 Jan  3 15:20 ./
drwxr-xr-x  3 dbrun dbrun  4096 Jan  3 15:18 ../
drwx------  5 dbrun dbrun  4096 Jan  3 15:20 base/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 global/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_commit_ts/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_dynshmem/
-rw-------  1 dbrun dbrun  4513 Jan  3 15:20 pg_hba.conf
-rw-------  1 dbrun dbrun  1636 Jan  3 15:20 pg_ident.conf
drwx------  4 dbrun dbrun  4096 Jan  3 15:20 pg_logical/
drwx------  4 dbrun dbrun  4096 Jan  3 15:20 pg_multixact/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_notify/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_replslot/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_serial/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_snapshots/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_stat/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_stat_tmp/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_subtrans/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_tblspc/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_twophase/
-rw-------  1 dbrun dbrun     3 Jan  3 15:20 PG_VERSION
drwx------  3 dbrun dbrun  4096 Jan  3 15:20 pg_wal/
drwx------  2 dbrun dbrun  4096 Jan  3 15:20 pg_xact/
-rw-------  1 dbrun dbrun    88 Jan  3 15:20 postgresql.auto.conf
-rw-------  1 dbrun dbrun 22764 Jan  3 15:20 postgresql.conf
dbrun@d:~/data$ 
```
