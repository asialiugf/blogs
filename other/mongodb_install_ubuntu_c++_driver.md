### mongodb安装
/etc/mongod.conf
```c
root@asiamiao:~# cd /etc
root@asiamiao:/etc# cat mongod.conf
# mongodb.conf

# Where to store the data.
dbpath=/data/mongo

#where to log
logpath=/var/log/mongodb/mongodb.log

logappend=true

bind_ip = 172.17.179.252,127.0.0.1
port = 27017

# Enable journaling, http://www.mongodb.org/display/DOCS/Journaling
journal=true

# Enables periodic logging of CPU utilization and I/O wait
#cpu = true

# Turn on/off security.  Off is currently the default
#noauth = true
#auth = true

# Verbose logging output.
#verbose = true

# Inspect all client data for validity on receipt (useful for
# developing drivers)
#objcheck = true

# Enable db quota management
#quota = true

# Set oplogging level where n is
#   0=off (default)
#   1=W
#   2=R
#   3=both
#   7=W+some reads
#oplog = 0

# Diagnostic/debugging option
#nocursors = true

# Ignore query hints
#nohints = true

# Disable the HTTP interface (Defaults to localhost:27018).
#nohttpinterface = true

# Turns off server-side scripting.  This will result in greatly limited
# functionality
#noscripting = true

# Turns off table scans.  Any query that would do a table scan fails.
#notablescan = true

# Disable data file preallocation.
#noprealloc = true

# Specify .ns file size for new databases.
# nssize = <size>

# Accout token for Mongo monitoring server.
#mms-token = <token>

# Server name for Mongo monitoring server.
#mms-name = <server-name>

# Ping interval for Mongo monitoring server.
#mms-interval = <seconds>

# Replication Options

# in replicated mongo databases, specify here whether this is a slave or master
#slave = true
#source = master.example.com
# Slave only: specify a single database to replicate
#only = master.example.com
# or
#master = true
#source = slave.example.com

# Address of a server to pair with.
#pairwith = <server:port>
# Address of arbiter server.
#arbiter = <server:port>
# Automatically resync if slave data is stale
#autoresync
# Custom size for replication operation log.
#oplogSize = <MB>
# Size limit for in-memory storage of op ids.
#opIdMem = <bytes>

# SSL options
# Enable SSL on normal ports
#sslOnNormalPorts = true
# SSL Key file and password
#sslPEMKeyFile = /etc/ssl/mongodb.pem
#sslPEMKeyPassword = pass
root@asiamiao:/etc# 
```

```c
root@d:/var/log/mongodb# systemctl unmask mongodb.service
root@d:/var/log/mongodb# chown -R mongodb:mongodb /data/mongo
root@d:/var/log/mongodb# systemctl start mongodb.service
root@d:/var/log/mongodb# service mongod start
root@d:/var/log/mongodb# service mongod stop
```
### Ubuntu16.04安装MongoDB Community 和 MongoDB C++ Driver
http://blog.csdn.net/u010821666/article/details/78069677

要注意，
- 先必须安装 MongoDB C Driver
```c
wget https://github.com/mongodb/mongo-c-driver/releases/download/1.9.0/mongo-c-driver-1.9.0.tar.gz
tar xvf mongo-c-driver-1.9.0.tar.gz
cd mongo-c-driver-1.9.0/
 ./configure
make
make install
```
- 再安装 MongoDB C++ Driver
```c
wget https://github.com/mongodb/mongo-cxx-driver/archive/r3.2.0-rc1.tar.gz
tar xvf r3.2.0-rc1.tar.gz
cd mongo-cxx-driver-r3.2.0-rc1/
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local ..
make EP_mnmlstc_core
make
make install
```
cmake的参数要加上，切记。

- 测试用例
```c
#include <iostream>

#include <bsoncxx/builder/stream/document.hpp>
#include <bsoncxx/json.hpp>

#include <mongocxx/client.hpp>
#include <mongocxx/instance.hpp>

int main(int, char**) {
    mongocxx::instance inst{};
    mongocxx::client conn{mongocxx::uri{}};

    bsoncxx::builder::stream::document document{};

    auto collection = conn["testdb"]["testcollection"];
    document << "hello" << "world";

    collection.insert_one(document.view());
    auto cursor = collection.find({});

    for (auto&& doc : cursor) {
        std::cout << bsoncxx::to_json(doc) << std::endl;
    }
}
```
- 编译方式：
```c
c++ --std=c++11 test.cpp -o test $(pkg-config --cflags --libs libmongocxx)
```
- 运行输出
```
root@d:/tmp# ./test
{ "_id" : { "$oid" : "5a488d553dc59722de5c4a32" }, "hello" : "world" }
root@d:/tmp# 
```
