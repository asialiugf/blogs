* https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/


#### Configure the package management system (yum).
Create a /etc/yum.repos.d/mongodb-org-4.0.repo
```c
[mongodb-org-4.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc
```
#### yum
```c
sudo yum install -y mongodb-org
```

```c
# mongo
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> 
> 
> exit
bye
[root@iZ23psatkqsZ etc]#  rpm -qa |grep mongodb
mongodb-org-shell-4.0.1-1.el7.x86_64
mongodb-org-server-4.0.1-1.el7.x86_64
mongodb-org-4.0.1-1.el7.x86_64
mongodb-org-mongos-4.0.1-1.el7.x86_64
mongodb-org-tools-4.0.1-1.el7.x86_64
[root@iZ23psatkqsZ etc]# 
[root@iZ23psatkqsZ etc]# rpm -ql mongodb-org-server
/etc/mongod.conf
/lib/systemd/system/mongod.service
/usr/bin/mongod
/usr/share/doc/mongodb-org-server-4.0.1
/usr/share/doc/mongodb-org-server-4.0.1/GNU-AGPL-3.0
/usr/share/doc/mongodb-org-server-4.0.1/LICENSE-Community.txt
/usr/share/doc/mongodb-org-server-4.0.1/MPL-2
/usr/share/doc/mongodb-org-server-4.0.1/README
/usr/share/doc/mongodb-org-server-4.0.1/THIRD-PARTY-NOTICES
/usr/share/man/man1/mongod.1
/var/lib/mongo
/var/log/mongodb
/var/log/mongodb/mongod.log
/var/run/mongodb
[root@iZ23psatkqsZ etc]# 
```
