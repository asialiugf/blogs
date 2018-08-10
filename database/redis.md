### 安装
用yum 安装
```c
在以下这个文件中
/etc/redis.conf
加上以下一句修改password!
requirepass foobarbook
然后重启！
# redis-server /etc/redis.conf &
```
```c
[root@iZ23psatkqsZ etc]# redis-cli
127.0.0.1:6379> auth foobarbook
OK
127.0.0.1:6379> 
```
