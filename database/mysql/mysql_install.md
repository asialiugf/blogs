### mysql install
* https://www.cnblogs.com/bigbrotherer/p/7241845.html
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

#### 安装 connector
```c++
[root@iZ23psatkqsZ ~]# wget https://cdn.mysql.com//Downloads/Connector-C++/mysql-connector-c++-8.0.12-1.el7.x86_64.rpm
--2018-08-03 13:42:33--  https://cdn.mysql.com//Downloads/Connector-C++/mysql-connector-c++-8.0.12-1.el7.x86_64.rpm
Resolving cdn.mysql.com (cdn.mysql.com)... 96.7.157.112
Connecting to cdn.mysql.com (cdn.mysql.com)|96.7.157.112|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 591368 (578K) [application/x-redhat-package-manager]
Saving to: ‘mysql-connector-c++-8.0.12-1.el7.x86_64.rpm’

100%[========================================================================================================>] 591,368      943KB/s   in 0.6s   

2018-08-03 13:42:35 (943 KB/s) - ‘mysql-connector-c++-8.0.12-1.el7.x86_64.rpm’ saved [591368/591368]

[root@iZ23psatkqsZ ~]# ll
total 584
drwxr-xr-x 6 root root   4096 Aug  2 00:23 github
-rw-r--r-- 1 root root 591368 Jun 27 13:10 mysql-connector-c++-8.0.12-1.el7.x86_64.rpm
[root@iZ23psatkqsZ ~]# yum install mysql-connector-c++-8.0.12-1.el7.x86_64.rpm 
Loaded plugins: fastestmirror
Examining mysql-connector-c++-8.0.12-1.el7.x86_64.rpm: mysql-connector-c++-8.0.12-1.el7.x86_64
Marking mysql-connector-c++-8.0.12-1.el7.x86_64.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package mysql-connector-c++.x86_64 0:8.0.12-1.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

==================================================================================================================================================
 Package                           Arch                 Version                      Repository                                              Size
==================================================================================================================================================
Installing:
 mysql-connector-c++               x86_64               8.0.12-1.el7                 /mysql-connector-c++-8.0.12-1.el7.x86_64               2.0 M

Transaction Summary
==================================================================================================================================================
Install  1 Package

Total size: 2.0 M
Installed size: 2.0 M
Is this ok [y/d/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : mysql-connector-c++-8.0.12-1.el7.x86_64                                                                                        1/1 
  Verifying  : mysql-connector-c++-8.0.12-1.el7.x86_64                                                                                        1/1 

Installed:
  mysql-connector-c++.x86_64 0:8.0.12-1.el7                                                                                                       

Complete!
[root@iZ23psatkqsZ ~]# 
```

### 安装开发环境xdevapi.h
* https://dev.mysql.com/doc/connectors/en/connector-cpp-installation-binary.html#connector-cpp-installation-binary-linux
```c++
[root@iZ23psatkqsZ /]# yum install mysql-connector-c++-devel
[root@iZ23psatkqsZ /]# yum install mysql-connector-c++-jdbc
[root@iZ23psatkqsZ /]# yum install mysql-connector-c++

mysql-connector-c++: This package provides the shared connector library implementing X DevAPI and X DevAPI for C.

mysql-connector-c++-jdbc: This package provides the shared legacy connector library implementing the JDBC API.

mysql-connector-c++-devel: This package installs development files required for building applications that use Connector/C++ libraries provided by the other packages, and static connector libraries. This package depends on the shared libraries provided by the other packages and cannot be installed by itself.
```
### connector-j-examples
* https://dev.mysql.com/doc/connectors/en/connector-j-examples.html
* https://dev.mysql.com/doc/connectors/en/
```c++
https://dev.mysql.com/doc/connectors/en/connector-j-examples.html
```

### build
编译请看这个文档：
* https://dev.mysql.com/doc/connectors/en/connector-cpp-apps-make.html

xdevapi例子请看这个文档：
* https://dev.mysql.com/doc/x-devapi-userguide/en/database-connection-example.html

例子，请看这个文档：
* https://dev.mysql.com/doc/dev/connector-cpp/8.0/devapi_ref.html

```c++
:::: [root@iZ23psatkqsZ mysql]# g++ -std=c++11 -I /usr/include/mysql-cppconn-8 -L .../lib64 test.cpp -lmysqlcppconn8 -o app    
/* ======== test.cpp ==========
 * Copyright (c) 2015, 2018, Oracle and/or its affiliates. All rights reserved.
 *
 * This program is free software; you can redistribute it and/or modify
 * it under the terms of the GNU General Public License, version 2.0, as
 * published by the Free Software Foundation.
 *
 * This program is also distributed with certain software (including
 * but not limited to OpenSSL) that is licensed under separate terms,
 * as designated in a particular file or component or in included license
 * documentation.  The authors of MySQL hereby grant you an
 * additional permission to link the program and your derivative works
 * with the separately licensed software that they have included with
 * MySQL.
 *
 * Without limiting anything contained in the foregoing, this file,
 * which is part of MySQL Connector/C++, is also subject to the
 * Universal FOSS Exception, version 1.0, a copy of which can be found at
 * http://oss.oracle.com/licenses/universal-foss-exception.
 *
 * This program is distributed in the hope that it will be useful, but
 * WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
 * See the GNU General Public License, version 2.0, for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program; if not, write to the Free Software Foundation, Inc.,
 * 51 Franklin St, Fifth Floor, Boston, MA 02110-1301  USA
 */
#include <iostream>
#include <mysqlx/xdevapi.h>
using ::std::cout;
using ::std::endl;
using namespace ::mysqlx;
int main(int argc, const char* argv[])
try {
  const char   *url = (argc > 1 ? argv[1] : "mysqlx://root@127.0.0.1");
  cout << "Creating session on " << url
       << " ..." << endl;
  Session sess(url);
  {
  /*
    TODO: Only working with server version 8
  */
    RowResult res = sess.sql("show variables like 'version'").execute();
    std::stringstream version;
    version << res.fetchOne().get(1).get<string>();
    int major_version;
    version >> major_version;
    if (major_version < 8)
    {
      cout <<"Done!" <<endl;
      return 0;
    }
  }
  cout <<"Session accepted, creating collection..." <<endl;
  Schema sch= sess.getSchema("test");
  Collection coll= sch.createCollection("c1", true);
  cout <<"Inserting documents..." <<endl;
  coll.remove("true").execute();
  {
    Result add;
    add= coll.add(R"({ "name": "foo", "age": 1 })").execute();
    std::vector<string> ids = add.getGeneratedIds();
    cout <<"- added doc with id: " << ids[0] <<endl;
    add= coll.add(R"({ "name": "bar", "age": 2, "toys": [ "car", "ball" ] })")
             .execute();
    if (ids.size() != 0)
      cout <<"- added doc with id: " << ids[0] <<endl;
    else
      cout <<"- added doc" <<endl;
    add= coll.add(R"({
       "name": "baz",
        "age": 3,
       "date": { "day": 20, "month": "Apr" }
    })").execute();
    if (ids.size() != 0)
      cout <<"- added doc with id: " << ids[0] <<endl;
    else
      cout <<"- added doc" <<endl;
    add= coll.add(R"({ "_id": "myuuid-1", "name": "foo", "age": 7 })")
             .execute();
    ids = add.getGeneratedIds();
    if (ids.size() != 0)
      cout <<"- added doc with id: " << ids[0] <<endl;
    else
      cout <<"- added doc" <<endl;
  }
  cout <<"Fetching documents..." <<endl;
  DocResult docs = coll.find("age > 1 and name like 'ba%'").execute();
  DbDoc doc = docs.fetchOne();
  for (int i = 0; doc; ++i, doc = docs.fetchOne())
  {
    cout <<"doc#" <<i <<": " <<doc <<endl;
    for (Field fld : doc)
    {
      cout << " field `" << fld << "`: " <<doc[fld] << endl;
    }
    string name = doc["name"];
    cout << " name: " << name << endl;
    if (doc.hasField("date") && Value::DOCUMENT == doc.fieldType("date"))
    {
      cout << "- date field" << endl;
      DbDoc date = doc["date"];
      for (Field fld : date)
      {
        cout << "  date `" << fld << "`: " << date[fld] << endl;
      }
      string month = doc["date"]["month"];
      int day = date["day"];
      cout << "  month: " << month << endl;
      cout << "  day: " << day << endl;
    }
    if (doc.hasField("toys") && Value::ARRAY == doc.fieldType("toys"))
    {
      cout << "- toys:" << endl;
      for (auto toy : doc["toys"])
      {
        cout << "  " << toy << endl;
      }
    }
    cout << endl;
  }
  cout <<"Done!" <<endl;
}
catch (const mysqlx::Error &err)
{
  cout <<"ERROR: " <<err <<endl;
  return 1;
}
catch (std::exception &ex)
{
  cout <<"STD EXCEPTION: " <<ex.what() <<endl;
  return 1;
}
catch (const char *ex)
{
  cout <<"EXCEPTION: " <<ex <<endl;
  return 1;
}
```


#### output
```c++
[root@localhost ~]# grep "password" /var/log/mysqld.log
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'HaHa1234YY89$';

创建数据库
[root@iZ23psatkqsZ /]# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 15
Server version: 8.0.12 MySQL Community Server - GPL

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
mysql> 
mysql> create database test;
Query OK, 1 row affected (0.10 sec)

mysql> 

```

* [root@iZ23psatkqsZ mysql]# ./app
```c++ 
[root@iZ23psatkqsZ mysql]# ./app
Creating session on mysqlx://root:HaHa1234YY89$@localhost:33060 ...
Session accepted, creating collection...
ERROR: CDK Error: Unknown database 'test'
[root@iZ23psatkqsZ mysql]# 
[root@iZ23psatkqsZ mysql]# ./app
Creating session on mysqlx://root:HaHa1234YY89$@localhost:33060 ...
Session accepted, creating collection...
Inserting documents...
- added doc with id: 00005b6345c00000000000000001
- added doc with id: 00005b6345c00000000000000001
- added doc with id: 00005b6345c00000000000000001
- added doc
Fetching documents...
doc#0: {"_id": "00005b6345c00000000000000002", "age": 2, "name": "bar", "toys": ["car", "ball"]}
 field `_id`: 00005b6345c00000000000000002
 field `age`: 2
 field `name`: bar
 field `toys`: <array with 2 element(s)>
 name: bar
- toys:
  car
  ball

doc#1: {"_id": "00005b6345c00000000000000003", "age": 3, "date": {"day": 20, "month": "Apr"}, "name": "baz"}
 field `_id`: 00005b6345c00000000000000003
 field `age`: 3
 field `date`: <document>
 field `name`: baz
 name: baz
- date field
  date `day`: 20
  date `month`: Apr
  month: Apr
  day: 20

Done!
[root@iZ23psatkqsZ mysql]# 
```

#### mysql shell
* https://dev.mysql.com/doc/mysql-shell/8.0/en/
```c++
[rabbit@iZ23psatkqsZ ~]$ 
[rabbit@iZ23psatkqsZ ~]$ mysqlsh
MySQL Shell 8.0.12

Copyright (c) 2016, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type '\help' or '\?' for help; '\quit' to exit.


 MySQL  JS > 
```










