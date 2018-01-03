###使用postgresql

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



####详细的内容如下：
```c
root@asiamiao:~# 
root@asiamiao:~# adduser dbrun
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
root@asiamiao:~# 
root@asiamiao:~# 
root@asiamiao:~# 
root@asiamiao:~# su - dbrun
dbrun@asiamiao:~$ ll
total 20
drwxr-xr-x  2 dbrun dbrun 4096 Jan  3 15:18 ./
drwxr-xr-x 10 root  root  4096 Jan  3 15:18 ../
-rw-r--r--  1 dbrun dbrun  220 Jan  3 15:18 .bash_logout
-rw-r--r--  1 dbrun dbrun 3771 Jan  3 15:18 .bashrc
-rw-r--r--  1 dbrun dbrun  655 Jan  3 15:18 .profile
dbrun@asiamiao:~$ 
dbrun@asiamiao:~$ 
dbrun@asiamiao:~$ 
dbrun@asiamiao:~$ 
dbrun@asiamiao:~$ 
dbrun@asiamiao:~$ 
dbrun@asiamiao:~$ 
dbrun@asiamiao:~$ pwd
/home/dbrun
dbrun@asiamiao:~$ 
dbrun@asiamiao:~$ mkdir data
dbrun@asiamiao:~$ ll
total 24
drwxr-xr-x  3 dbrun dbrun 4096 Jan  3 15:18 ./
drwxr-xr-x 10 root  root  4096 Jan  3 15:18 ../
-rw-r--r--  1 dbrun dbrun  220 Jan  3 15:18 .bash_logout
-rw-r--r--  1 dbrun dbrun 3771 Jan  3 15:18 .bashrc
drwxrwxr-x  2 dbrun dbrun 4096 Jan  3 15:18 data/
-rw-r--r--  1 dbrun dbrun  655 Jan  3 15:18 .profile
dbrun@asiamiao:~$ pwd
/home/dbrun
dbrun@asiamiao:~$ cd data
dbrun@asiamiao:~/data$ ll
total 8
drwxrwxr-x 2 dbrun dbrun 4096 Jan  3 15:18 ./
drwxr-xr-x 3 dbrun dbrun 4096 Jan  3 15:18 ../
dbrun@asiamiao:~/data$ pwd
/home/dbrun/data
dbrun@asiamiao:~/data$ 
dbrun@asiamiao:~/data$ 
dbrun@asiamiao:~/data$ 
dbrun@asiamiao:~/data$ /usr/local/pgsql/bin/initdb -D .
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

dbrun@asiamiao:~/data$ 
dbrun@asiamiao:~/data$ 
dbrun@asiamiao:~/data$ ll
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
dbrun@asiamiao:~/data$ 
```
