### postgresql安装

```
https://ftp.postgresql.org/pub/source/v10.1/postgresql-10.1.tar.bz2
root@d:~/source# tar xvf postgresql-10.1.tar.bz2 
root@d:~/source/postgresql-10.1# apt-get install libreadline-gplv2-dev
root@d:~/source/postgresql-10.1# ./configure
root@d:~/source/postgresql-10.1# make
root@d:~/source/postgresql-10.1# make install
root@d:~# adduser postgres

root@d:~/source/postgresql-10.1# mkdir /usr/local/pgsql/data
root@d:~/source/postgresql-10.1# chown postgres /usr/local/pgsql/data
root@d:~/source/postgresql-10.1# su - postgres
postgres@d:~$ cd /usr/local/pgsql/data
postgres@d:/usr/local/pgsql/data$ /usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data  
postgres@d:/usr/local/pgsql/data$ /usr/local/pgsql/bin/postgres -D /usr/local/pgsql/data >logfile 2>&1 &
postgres@d:/usr/local/pgsql/data$ /usr/local/pgsql/bin/createdb test

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
