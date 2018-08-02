### mysql install
```c++
[root@iZ23psatkqsZ rabbit]# wget https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm
--2018-08-03 01:49:48--  https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm
Resolving repo.mysql.com (repo.mysql.com)... 23.218.45.194
Connecting to repo.mysql.com (repo.mysql.com)|23.218.45.194|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 25820 (25K) [application/x-redhat-package-manager]
Saving to: ‘mysql80-community-release-el7-1.noarch.rpm’

100%[=============================================================================>] 25,820      --.-K/s   in 0.06s   

2018-08-03 01:49:49 (420 KB/s) - ‘mysql80-community-release-el7-1.noarch.rpm’ saved [25820/25820]

[root@iZ23psatkqsZ rabbit]# 
[root@iZ23psatkqsZ rabbit]# ll
total 357316
-rw-r--r--  1 root   root       25820 Apr 18 13:24 mysql80-community-release-el7-1.noarch.rpm
-rw-rw-r--  1 rabbit rabbit 365857924 Jun 29 16:55 mysql-community-server-8.0.12-1.el7.x86_64.rpm
drwxrwxr-x 18 rabbit rabbit      4096 Aug  2 10:57 uquant
[root@iZ23psatkqsZ rabbit]# yum -y install mysql80-community-release-el7-1.noarch.rpm
Loaded plugins: fastestmirror
Examining mysql80-community-release-el7-1.noarch.rpm: mysql80-community-release-el7-1.noarch
Marking mysql80-community-release-el7-1.noarch.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package mysql80-community-release.noarch 0:el7-1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=======================================================================================================================
 Package                           Arch           Version        Repository                                       Size
=======================================================================================================================
Installing:
 mysql80-community-release         noarch         el7-1          /mysql80-community-release-el7-1.noarch          31 k

Transaction Summary
=======================================================================================================================
Install  1 Package

Total size: 31 k
Installed size: 31 k
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : mysql80-community-release-el7-1.noarch                                                              1/1 
  Verifying  : mysql80-community-release-el7-1.noarch                                                              1/1 

Installed:
  mysql80-community-release.noarch 0:el7-1                                                                             

Complete!
[root@iZ23psatkqsZ rabbit]# 
[root@iZ23psatkqsZ rabbit]# yum -y install mysql-community-server
Loaded plugins: fastestmirror
mysql-connectors-community                                                                      | 2.5 kB  00:00:00     
mysql-tools-community                                                                           | 2.5 kB  00:00:00     
mysql80-community                                                                               | 2.5 kB  00:00:00     
(1/3): mysql-connectors-community/x86_64/primary_db                                             |  25 kB  00:00:00     
(2/3): mysql80-community/x86_64/primary_db                                                      |  26 kB  00:00:00     
(3/3): mysql-tools-community/x86_64/primary_db                                                  |  45 kB  00:00:00     
Loading mirror speeds from cached hostfile
Resolving Dependencies
--> Running transaction check
---> Package mysql-community-server.x86_64 0:8.0.12-1.el7 will be installed
--> Processing Dependency: mysql-community-common(x86-64) = 8.0.12-1.el7 for package: mysql-community-server-8.0.12-1.el7.x86_64
--> Processing Dependency: mysql-community-client(x86-64) >= 8.0.0 for package: mysql-community-server-8.0.12-1.el7.x86_64
--> Processing Dependency: libaio.so.1(LIBAIO_0.4)(64bit) for package: mysql-community-server-8.0.12-1.el7.x86_64
--> Processing Dependency: libaio.so.1(LIBAIO_0.1)(64bit) for package: mysql-community-server-8.0.12-1.el7.x86_64
--> Processing Dependency: libaio.so.1()(64bit) for package: mysql-community-server-8.0.12-1.el7.x86_64
--> Running transaction check
---> Package libaio.x86_64 0:0.3.109-13.el7 will be installed
---> Package mysql-community-client.x86_64 0:8.0.12-1.el7 will be installed
--> Processing Dependency: mysql-community-libs(x86-64) >= 8.0.0 for package: mysql-community-client-8.0.12-1.el7.x86_64
---> Package mysql-community-common.x86_64 0:8.0.12-1.el7 will be installed
--> Running transaction check
---> Package mariadb-libs.x86_64 1:5.5.56-2.el7 will be obsoleted
--> Processing Dependency: libmysqlclient.so.18()(64bit) for package: 2:postfix-2.10.1-6.el7.x86_64
--> Processing Dependency: libmysqlclient.so.18(libmysqlclient_18)(64bit) for package: 2:postfix-2.10.1-6.el7.x86_64
---> Package mysql-community-libs.x86_64 0:8.0.12-1.el7 will be obsoleting
--> Running transaction check
---> Package mysql-community-libs-compat.x86_64 0:8.0.12-1.el7 will be obsoleting
--> Finished Dependency Resolution

Dependencies Resolved

=======================================================================================================================
 Package                                Arch              Version                   Repository                    Size
=======================================================================================================================
Installing:
 mysql-community-libs                   x86_64            8.0.12-1.el7              mysql80-community            2.2 M
     replacing  mariadb-libs.x86_64 1:5.5.56-2.el7
 mysql-community-libs-compat            x86_64            8.0.12-1.el7              mysql80-community            2.1 M
     replacing  mariadb-libs.x86_64 1:5.5.56-2.el7
 mysql-community-server                 x86_64            8.0.12-1.el7              mysql80-community            349 M
Installing for dependencies:
 libaio                                 x86_64            0.3.109-13.el7            base                          24 k
 mysql-community-client                 x86_64            8.0.12-1.el7              mysql80-community             26 M
 mysql-community-common                 x86_64            8.0.12-1.el7              mysql80-community            541 k

Transaction Summary
=======================================================================================================================
Install  3 Packages (+3 Dependent packages)

Total download size: 379 M
Downloading packages:
(1/6): libaio-0.3.109-13.el7.x86_64.rpm                                                         |  24 kB  00:00:00     
warning: /var/cache/yum/x86_64/7/mysql80-community/packages/mysql-community-common-8.0.12-1.el7.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Public key for mysql-community-common-8.0.12-1.el7.x86_64.rpm is not installed
(2/6): mysql-community-common-8.0.12-1.el7.x86_64.rpm                                           | 541 kB  00:00:00     
(3/6): mysql-community-libs-8.0.12-1.el7.x86_64.rpm                                             | 2.2 MB  00:00:00     
(4/6): mysql-community-libs-compat-8.0.12-1.el7.x86_64.rpm                                      | 2.1 MB  00:00:00     
(5/6): mysql-community-client-8.0.12-1.el7.x86_64.rpm                                           |  26 MB  00:00:01     
(6/6): mysql-community-server-8.0.12-1.el7.x86_64.rpm                                           | 349 MB  00:00:16     
-----------------------------------------------------------------------------------------------------------------------
Total                                                                                   21 MB/s | 379 MB  00:00:17     
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
Importing GPG key 0x5072E1F5:
 Userid     : "MySQL Release Engineering <mysql-build@oss.oracle.com>"
 Fingerprint: a4a9 4068 76fc bd3c 4567 70c8 8c71 8d3b 5072 e1f5
 Package    : mysql80-community-release-el7-1.noarch (installed)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : mysql-community-common-8.0.12-1.el7.x86_64                                                          1/7 
  Installing : mysql-community-libs-8.0.12-1.el7.x86_64                                                            2/7 
  Installing : mysql-community-client-8.0.12-1.el7.x86_64                                                          3/7 
  Installing : libaio-0.3.109-13.el7.x86_64                                                                        4/7 
  Installing : mysql-community-server-8.0.12-1.el7.x86_64                                                          5/7 
  Installing : mysql-community-libs-compat-8.0.12-1.el7.x86_64                                                     6/7 
  Erasing    : 1:mariadb-libs-5.5.56-2.el7.x86_64                                                                  7/7 
  Verifying  : mysql-community-libs-8.0.12-1.el7.x86_64                                                            1/7 
  Verifying  : mysql-community-libs-compat-8.0.12-1.el7.x86_64                                                     2/7 
  Verifying  : mysql-community-common-8.0.12-1.el7.x86_64                                                          3/7 
  Verifying  : mysql-community-server-8.0.12-1.el7.x86_64                                                          4/7 
  Verifying  : mysql-community-client-8.0.12-1.el7.x86_64                                                          5/7 
  Verifying  : libaio-0.3.109-13.el7.x86_64                                                                        6/7 
  Verifying  : 1:mariadb-libs-5.5.56-2.el7.x86_64                                                                  7/7 

Installed:
  mysql-community-libs.x86_64 0:8.0.12-1.el7              mysql-community-libs-compat.x86_64 0:8.0.12-1.el7           
  mysql-community-server.x86_64 0:8.0.12-1.el7           

Dependency Installed:
  libaio.x86_64 0:0.3.109-13.el7                             mysql-community-client.x86_64 0:8.0.12-1.el7              
  mysql-community-common.x86_64 0:8.0.12-1.el7              

Replaced:
  mariadb-libs.x86_64 1:5.5.56-2.el7                                                                                   

Complete!
[root@iZ23psatkqsZ rabbit]# 
[root@iZ23psatkqsZ rabbit]# 
[root@iZ23psatkqsZ rabbit]# 
[root@iZ23psatkqsZ rabbit]# which mysql
/bin/mysql
[root@iZ23psatkqsZ rabbit]# 
```
### 开启服务
```c++
[root@iZ23psatkqsZ rabbit]# 
[root@iZ23psatkqsZ rabbit]# systemctl start  mysqld.service
[root@iZ23psatkqsZ rabbit]# systemctl status mysqld.service
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2018-08-03 01:56:16 CST; 5s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 19515 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 19583 (mysqld)
   Status: "SERVER_OPERATING"
   CGroup: /system.slice/mysqld.service
           └─19583 /usr/sbin/mysqld

Aug 03 01:55:59 iZ23psatkqsZ systemd[1]: Starting MySQL Server...
Aug 03 01:56:16 iZ23psatkqsZ systemd[1]: Started MySQL Server.
[root@iZ23psatkqsZ rabbit]# 
```
### 安装 开发包
```c++
[root@iZ23psatkqsZ /]#  yum install mysql-devel -y 
[root@iZ23psatkqsZ /]# find . -name "mysql.h"
./usr/include/mysql/mysql.h
```
### 连接池
* https://dev.mysql.com/downloads/connector/cpp/
* https://www.cnblogs.com/lianshuiwuyi/p/8592870.html
* https://blog.csdn.net/chenxun_2010/article/details/68954212
#### X DevAPI User Guide
* https://dev.mysql.com/doc/x-devapi-userguide/en/database-connection-example.html


