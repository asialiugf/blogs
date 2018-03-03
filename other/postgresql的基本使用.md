### 使用postgresql

#### 【基本概念】

- 一个linux的用户，可以有多个目录用来初始化postgresql。
- 一个被postgresql初始化的目录，对应一个postgresql的服务，必须有独立的IP:PORT。
- 一个postgresql服务，有一组相应的进程。
- 通过不同的的IP:PORT对，可以在一个linux上开启多组postgresql服务。
- 一个物理目录，对应一个服务，一个服务，可以创建多个database。
- 一个角色可以管理多个database。
- 一个database也可以被多个角色管理  M:N关系。
- 一个用户，是具体登录权限的角角色。


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
/usr/local/pgsql/bin/createdb -p 6688 future --username postgres
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

#### 登录数据库
第一个参数future是数据库，第二个参数dbrun是角色，第三个参数指定port，第四个参数指定服务的IP。
```
riddle@asiamiao:~$ /usr/local/pgsql/bin/psql future dbrun --port=6688 --host=127.0.0.2
dbrun@asiamiao:~$ /usr/local/pgsql/bin/psql -p 6688 future dbrun 
dbrun@asiamiao:~$ /usr/local/pgsql/bin/psql -p 6688 postgres dbrun
```

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
#### 角色和用户的概念
【结论】
- 用户就是角色。
- 用户是带有登录权限的角色。
- CREATE USER is now an alias for CREATE ROLE.

https://www.postgresql.org/docs/10/static/user-manag.html
http://blog.csdn.net/TTchengcheng/article/details/78881789
最近在学数据库，老师要求我们通过使用postgresql来学习。拿着用SQL server演示的教材，看着SQL server的实验指导书，在Postgresql上做实现，虽说都是关系型数据库，但是很多概念都是不一样的，真的是相当难受。使用postgresql的人在国内比较少，在学习访问权限的时候遇到很多问题，在度娘上没找到答案，只好自己查阅了官方文档，经过大量的实践才算略懂一二。在此分享给大家，说的很啰嗦，请谅解。注下面的实践都是在PostgreSQL 9.5上完成的，图形化管理软件为pgAdmin III。

Postgresql的官方文档上对于role的定义是A role is an entity that can own database objectsand have database privileges; a role can be considered a "user",a "group", or both depending on how it isused.意思是一个角色是一个可以有自己的数据库对象和数据库操纵权限的实体,一个角色可以被认为是一个“用户”,一个“组”,或者两者都可，取决于它的使用方式。

Postgresql的官方文档上对于CREATE USER命令的说明是CREATEUSER is now an alias forCREATEROLE. The only difference is that when the command is spelled CREATE USER,LOGIN is assumed by default,whereas NOLOGIN is assumed whenthe command is spelledCREATEROLE.也就是说CREATE USER实际上就是CREATE ROLE，唯一不同的是， CREATE USER命令默认是带有登录权限的，而CREATE ROLE则没有。

而所谓的登录角色和组角色在似乎是pgAdmin所定义的，我官方文档上并没有找到login role或者group role的定义，不过我找到这样一条说明The concept of rolessubsumes the concepts of "users" and "groups". In PostgreSQLversions before 8.1, users and groups were distinct kinds of entities, but nowthere are only roles. Any role can act as a user, a group, or both.

大家自己理解把。（看这意思似乎是8.1版之前存在过users和groups的概念，不过现在都是role了）

总的来说，登录角色就是具有登录权限的角色，是通常意义上的用户，不具有登录权限的角色就是组角色，是一些登录角色的集合。这样的目的是为了方便批量授权。在PostgreSQL中登录角色、组角色和用户本质上都是角色。


#### 查看角色
```
postgres=# \du
                                   List of roles
 Role name |                         Attributes                         | Member of 
-----------+------------------------------------------------------------+-----------
 dbrun     | Superuser, Create role, Create DB, Replication, Bypass RLS | {}

postgres=# 
```

#### 查看角色
```
postgres=# select rolname from pg_roles;
       rolname        
----------------------
 dbrun
 pg_monitor
 pg_read_all_settings
 pg_read_all_stats
 pg_stat_scan_tables
 pg_signal_backend
(6 rows)
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
#### 查看表
```
\dt

future=# 
future=# CREATE TABLE kkk (
future(#     datetime        VARCHAR(10) NOT NULL,
future(#     ms              VARCHAR(10) NOT NULL,
future(#     serials         VARCHAR UNIQUE NOT NULL,
future(#     trading_day     VARCHAR(10),
future(#     highest         DOUBLE PRECISION NOT NULL,
future(#     lowest          DOUBLE PRECISION NOT NULL,
future(#     last_price      DOUBLE PRECISION NOT NULL,
future(#     ask_price1      DOUBLE PRECISION NOT NULL,
future(#     ask_volume1     DOUBLE PRECISION NOT NULL,
future(#     bid_price1      DOUBLE PRECISION NOT NULL,
future(#     bid_volume1     DOUBLE PRECISION NOT NULL,
future(#     open_interest   DOUBLE PRECISION NOT NULL,
future(#     volume          DOUBLE PRECISION NOT NULL,
future(#     PRIMARY KEY (datetime, ms)       
future(# ); 
CREATE TABLE
future=# 
future=# \d
        List of relations
 Schema | Name | Type  |  Owner   
--------+------+-------+----------
 public | kkk  | table | postgres
(1 row)

future=# 
```
#### 查看表结构  \d 表名
```
future=# \d kkk
                           Table "public.kkk"
    Column     |         Type          | Collation | Nullable | Default 
---------------+-----------------------+-----------+----------+---------
 datetime      | character varying(10) |           | not null | 
 ms            | character varying(10) |           | not null | 
 serials       | character varying     |           | not null | 
 trading_day   | character varying(10) |           |          | 
 highest       | double precision      |           | not null | 
 lowest        | double precision      |           | not null | 
 last_price    | double precision      |           | not null | 
 ask_price1    | double precision      |           | not null | 
 ask_volume1   | double precision      |           | not null | 
 bid_price1    | double precision      |           | not null | 
 bid_volume1   | double precision      |           | not null | 
 open_interest | double precision      |           | not null | 
 volume        | double precision      |           | not null | 
Indexes:
    "kkk_pkey" PRIMARY KEY, btree (datetime, ms)
    "kkk_serials_key" UNIQUE CONSTRAINT, btree (serials)

future=# 
```
