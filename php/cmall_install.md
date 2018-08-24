### nginx
```
/usr/local/nginx/conf
```
```
    upstream php5.4 {
      server   unix:/dev/shm/php-fpm/php-fpm.sock;
    }

    upstream php5.3 {
      server   unix:/dev/shm/php-fpm/php-fpm5.3.sock;
    }
```
### php_
```
/usr/local/php/etc
```
```
/usr/local/php/etc
[root@localhost etc]# ll
total 172
-rw-r--r--. 1 root root  1152 Mar 13  2015 pear.conf
-rw-r--r--. 1 root root 21975 Mar 16  2015 php-fpm1.conf
-rw-r--r--. 1 root root 21972 Mar 16  2015 php-fpm20170919.conf
-rw-r--r--  1 root root 22016 Sep 19  2017 php-fpm.conf
-rw-r--r--. 1 root root 22236 Mar 13  2015 php-fpm.conf.default
-rw-r--r--. 1 root root 65817 Mar 28  2016 php.ini
[root@localhost etc]# 
```
```
[root@localhost bin]# ll
total 70644
-rwxr-xr-x. 1 root root      837 Mar 13  2015 pear
-rwxr-xr-x. 1 root root      858 Mar 13  2015 peardev
-rwxr-xr-x. 1 root root      774 Mar 13  2015 pecl
lrwxrwxrwx. 1 root root       28 Sep 10  2016 phar -> /usr/local/php/bin/phar.phar
-rwxr-xr-x. 1 root root    14827 Mar 13  2015 phar.phar
-rwxr-xr-x. 1 root root 36136339 Mar 13  2015 php
-rwxr-xr-x. 1 root root 36074976 Mar 13  2015 php-cgi
-rwxr-xr-x. 1 root root     3171 Mar 13  2015 php-config
-rwxr-xr-x. 1 root root     4534 Mar 13  2015 phpize
[root@localhost bin]# 
[root@localhost bin]# cd ..
[root@localhost php]# ll
total 28
drwxr-xr-x. 2 root root 4096 Mar 13  2015 bin
drwxr-xr-x. 2 root root 4096 Sep 19  2017 etc
drwxr-xr-x. 3 root root 4096 Mar 13  2015 include
drwxr-xr-x. 3 root root 4096 Mar 13  2015 lib
drwxr-xr-x. 4 root root 4096 Mar 13  2015 php
drwxr-xr-x. 2 root root 4096 Mar 13  2015 sbin
drwxr-xr-x. 4 root root 4096 Mar 13  2015 var
[root@localhost php]# cd sbin
[root@localhost sbin]# ll
total 35760
-rwxr-xr-x. 1 root root 36573466 Mar 13  2015 php-fpm
[root@localhost sbin]# 
[root@localhost sbin]# 
[root@localhost sbin]# pwd
/usr/local/php/sbin
[root@localhost sbin]#
```
### memcached
```
[root@localhost ~]# 
[root@localhost ~]# ps -ef | grep memca
root     19837 19749  0 10:23 pts/4    00:00:00 grep memca
root     30923     1  0 Feb27 ?        00:08:12 /usr/bin/memcached -b -l 127.0.0.1 -p 11211 -m 150 -u root
[root@localhost ~]# 
```

### /usr/local
```
[root@localhost bin]# cd /usr/local
[root@localhost local]# ll
total 84
drwxr-xr-x.  2 root root 4096 Sep 12  2017 bin
drwxr-xr-x.  5 root root 4096 Sep 20  2014 cronolog
drwxr-xr-x.  2 root root 4096 Sep 23  2011 etc
drwxr-xr-x.  2 root root 4096 Sep 23  2011 games
drwxr-xr-x.  7 root root 4096 Mar 13  2015 imagemagick
drwxr-xr-x.  2 root root 4096 Sep 23  2011 include
drwxr-xr-x.  2 root root 4096 Sep 23  2011 lib
drwxr-xr-x.  2 root root 4096 Sep 23  2011 lib64
drwxr-xr-x.  2 root root 4096 Sep 23  2011 libexec
drwxr-xr-x.  6 root root 4096 Mar 13  2015 libiconv
drwxr-xr-x.  7 root root 4096 Mar 13  2015 libmcrypt
drwxr-xr-x.  5 root root 4096 Mar 13  2015 memcached
drwxr-xr-x   3 root root 4096 Sep 13  2017 mysql
drwxr-xr-x.  8 root root 4096 Sep 22  2017 nginx
drwxr-xr-x.  9 root root 4096 Mar 16  2015 php
drwxr-xr-x. 10 root root 4096 Jan  5  2016 php5.3
drwxr-xr-x. 10 root root 4096 Sep 20  2014 proftpd
drwxr-xr-x.  2 root root 4096 Sep 23  2011 sbin
drwxr-xr-x.  6 root root 4096 Mar 13  2015 scws
drwxr-xr-x.  5 root root 4096 Jan 17  2018 share
drwxr-xr-x.  3 root root 4096 Feb 27 13:57 src
[root@localhost local]# 
```

### mysql install
```
wget https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.23-el7-x86_64.tar.gz
tar xvf mysql-5.7.23-el7-x86_64.tar.gz
mv mysql-5.7.23-el7-x86_64 /usr/local/mysql5.7.23
```
