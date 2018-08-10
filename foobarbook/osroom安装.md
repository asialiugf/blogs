### 下载osroom
* https://github.com/osroom/osroom
```c
git clone https://github.com/osroom/osroom.git
```
```c++
# cd osroom/apps
/root/github/osroom/apps/configs
[root@iZ23psatkqsZ configs]# 
[root@iZ23psatkqsZ configs]# ll
total 92
-rw-r--r-- 1 root   root   30526 Aug 10 23:33 config.py
-rw-r--r-- 1 197608 197121 31394 Aug 10 02:27 config_sample.py
-rw-r--r-- 1 root   root    1232 Aug 10 23:28 db_config.py
-rw-r--r-- 1 197608 197121  1253 Aug 10 02:27 db_config_sample.py
-rw-r--r-- 1 197608 197121    48 Aug 10 02:27 __init__.py
-rw-r--r-- 1 197608 197121  1469 Aug 10 02:27 mdb_collection.py
drwxr-xr-x 2 root   root    4096 Aug 10 23:33 __pycache__
-rwxr-xr-x 1 197608 197121  4108 Aug 10 02:27 sys_config.py
[root@iZ23psatkqsZ configs]#
```

### mongodb redis 配置
```python
[root@iZ23psatkqsZ configs]# cat db_config.py
# -*-coding:utf-8-*-
__author__ = "Allen Woo"
DB_CONFIG = {
    "mongodb": {
        "mongo_sys": {
            "username": "myadmin",
            "config": {
                "fsync": False,
                "replica_set": None
            },
            "password": "secret",
            "dbname": "osr_sys",
            "host": [
                "127.0.0.1:27017"
            ]
        },
        "mongo_user": {
            "username": "myadmin",
            "config": {
                "fsync": False,
                "replica_set": None
            },
            "password": "secret",
            "dbname": "osr_user",
            "host": [
                "127.0.0.1:27017"
            ]
        },
        "mongo_web": {
            "username": "myadmin",
            "config": {
                "fsync": False,
                "replica_set": None
            },
            "password": "secret",
            "dbname": "osr_web",
            "host": [
                "127.0.0.1:27017"
            ]
        }
    },
    "redis": {
        "password": "foobarbook",
        "port": [
            "6379"
        ],
        "host": [
            "127.0.0.1"
        ]
    }
}
[root@iZ23psatkqsZ configs]# 
```


### mongodb配置
```c++
[root@iZ23psatkqsZ osroom]# mongo
MongoDB shell version v4.0.1
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 4.0.1
Server has startup warnings: 
2018-08-10T22:16:15.935+0800 I STORAGE  [initandlisten] 
2018-08-10T22:16:15.935+0800 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2018-08-10T22:16:15.935+0800 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2018-08-10T22:16:16.942+0800 I CONTROL  [initandlisten] 
2018-08-10T22:16:16.942+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2018-08-10T22:16:16.942+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] 
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] 
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] 
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] 
---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display
metrics about your deployment (disk utilization, CPU, operation statistics, etc).

The monitoring data will be available on a MongoDB website with a unique URL accessible to you
and anyone you share the URL with. MongoDB may use this information to make product
improvements and to suggest MongoDB products and deployment options to you.

To enable free monitoring, run the following command: db.enableFreeMonitoring()
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---

> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> 
> use osr_sys;
switched to db osr_sys
> db.createUser({ user: "root", pwd: "foobarbook", roles: [{ role: "userAdminAnyDatabase", db: "osr_sys" }] })
2018-08-10T23:15:16.463+0800 E QUERY    [js] Error: couldn't add user: No role named userAdminAnyDatabase@osr_sys :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
DB.prototype.createUser@src/mongo/shell/db.js:1491:15
@(shell):1:1
> 
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> 
> use osr_sys
switched to db osr_sys
>  db.createUser({ user: "root", pwd: "foobarbook");
... 
... ;
... 
... 
> 
> 
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> use osr_sys
switched to db osr_sys
> 
> db.addUser({ user: "root", pwd: "foobarbook"})
2018-08-10T23:16:55.465+0800 E QUERY    [js] TypeError: db.addUser is not a function :
@(shell):1:1
> 
> db.createUser({ user: "root", pwd: "foobarbook"})
2018-08-10T23:17:07.323+0800 E QUERY    [js] Error: couldn't add user: "createUser" command requires a "roles" array :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
DB.prototype.createUser@src/mongo/shell/db.js:1491:15
@(shell):1:1
> db.addUser({ user: "root", pwd: "foobarbook"})
2018-08-10T23:18:45.050+0800 E QUERY    [js] TypeError: db.addUser is not a function :
@(shell):1:1
> 
> 
> 
> 
> use tt
switched to db tt
> db.addUser('mongodb','123456');
2018-08-10T23:18:48.311+0800 E QUERY    [js] TypeError: db.addUser is not a function :
@(shell):1:1
> 
> 
> use osr_sys
switched to db osr_sys
> db.createUser(
...      {
...        user:"myadmin",
...        pwd:"secret",
...        roles:[{role:"root",db:"admin"}]
...      }
...   )
Successfully added user: {
        "user" : "myadmin",
        "roles" : [
                {
                        "role" : "root",
                        "db" : "admin"
                }
        ]
}
> 
> 
> 
> use osr_user
switched to db osr_user
> db.createUser(
...      {
...        user:"myadmin",
...        pwd:"secret",
...        roles:[{role:"root",db:"admin"}]
...      }
...   )
Successfully added user: {
        "user" : "myadmin",
        "roles" : [
                {
                        "role" : "root",
                        "db" : "admin"
                }
        ]
}
> show uses
2018-08-10T23:22:44.945+0800 E QUERY    [js] Error: don't know how to show [uses] :
shellHelper.show@src/mongo/shell/utils.js:1055:11
shellHelper@src/mongo/shell/utils.js:766:15
@(shellhelp2):1:1
> 
> use osr_user
switched to db osr_user
> show uses
2018-08-10T23:23:33.479+0800 E QUERY    [js] Error: don't know how to show [uses] :
shellHelper.show@src/mongo/shell/utils.js:1055:11
shellHelper@src/mongo/shell/utils.js:766:15
@(shellhelp2):1:1
> 
> show use
2018-08-10T23:23:36.636+0800 E QUERY    [js] Error: don't know how to show [use] :
shellHelper.show@src/mongo/shell/utils.js:1055:11
shellHelper@src/mongo/shell/utils.js:766:15
@(shellhelp2):1:1
> 
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> 
> 
> 
> use admin 
switched to db admin
> db.createUser(
...      {
...        user:"myadmin",
...        pwd:"secret",
...        roles:[{role:"root",db:"admin"}]
...      }
...   )
Successfully added user: {
        "user" : "myadmin",
        "roles" : [
                {
                        "role" : "root",
                        "db" : "admin"
                }
        ]
}
# # 查
2018-08-10T23:25:28.211+0800 E QUERY    [js] SyntaxError: illegal character @(shell):1:0
> 
> 
> show users
{
        "_id" : "admin.myadmin",
        "user" : "myadmin",
        "db" : "admin",
        "roles" : [
                {
                        "role" : "root",
                        "db" : "admin"
                }
        ],
        "mechanisms" : [
                "SCRAM-SHA-1",
                "SCRAM-SHA-256"
        ]
}
> 
> 
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> db.auth('myadmin', 'secret')
1
> use osr_web
switched to db osr_web
> db.createUser(
...      {
...        user:"myadmin",
...        pwd:"secret",
...        roles:[{role:"root",db:"admin"}]
...      }
...   )
# Successfully added user: {
        "user" : "myadmin",
        "roles" : [
                {
                        "role" : "root",
                        "db" : "admin"
                }
        ]
}
> # 
2018-08-10T23:28:50.084+0800 E QUERY    [js] SyntaxError: illegal character @(shell):1:0
> 
```
