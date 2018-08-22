### IPv4 forwarding is disabled
```
创建docker registry的时候报错：

[root@master ~]# docker run --name registry_server -d -p 5000:5000 -v /registry:/tmp/registry registry
WARNING: IPv4 forwarding is disabled. Networking will not work.
a93d14f536d31b3edfd847f12c8e1115bb75bc2b202fb44f761c2e14c2018a6b


解决办法：
# vi /etc/sysctl.conf
或者
# vi /usr/lib/sysctl.d/00-system.conf
添加如下代码：
    net.ipv4.ip_forward=1

重启network服务
# systemctl restart network

查看是否修改成功
# sysctl net.ipv4.ip_forward

如果返回为“net.ipv4.ip_forward = 1”则表示成功了
```
