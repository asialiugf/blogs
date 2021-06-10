# u盘启动盘制作
https://github.com/asialiugf/blogs/edit/master/linux/%E9%87%8D%E8%A3%85ubuntu.md
如果不用UEFI模式的启动盘，很可能导致你安装的Ubuntu并不能使用UEFI模式启动。
本文分两个部分

制作UEFI启动模式的启动盘
设置UEFI启动
1. 制作UEFI启动模式的启动盘
ubuntu官方USB启动盘制作文章（on windows） 
http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows

然而，上面的文章并不足以让你创造出一个完整的UEFI启动U盘。 
并不推荐在ubuntu下创建启动盘，已知问题是12.04和14.04创建15.06的启动盘会出错，但是windows下管你什么直接都能创建成功。

制作准备工具：

容量至少4G的U盘（内容会被清空，请备份资料）
Ubuntu 64位 安装iso （可从官网下载需要的版本）
Rufus U盘工具（下载地址： https://rufus.akeo.ie/ ）
制作步骤：

Rufus下载完直接运行即可，我下载的版本是2.8（Last updated 2016.03.22）。Rufus支持中文，感谢汉化人员。
插入U盘，Rufus会自动识别U盘，如果插入多只，请自行选择。
与官方教程不一样的是，分区模式选用UEFI下的MBR。 

# 设置 sudo 用户
```
vi /etc/sudoers
添加以下两行，将heaven用户设置为 sudo 用户。
heaven  ALL=(ALL:ALL) ALL
%sudo   ALL=(ALL:ALL) NOPASSWD: ALL
```


# 各种安装包
```c++
apt-get update
apt-get upgrade
apt-get install vim-gui-common  
apt-get install vim-runtime  
apt-get install git
apt-get install libssl-dev
apt-get install lib32ncurses5 lib32z1
apt-get install cmake
apt-get install uuid-dev


```
# vim安装
在普通用下：
```c++
tar xvf vim.tar.gz
```
# git 
在相关的项目目录下执行以下命令。
```c++
git config credential.helper store
```

# uWS 安装
```c++
 git clone https://github.com/uNetworking/uWebSockets.git
 cd uWebsockets
 make
 make install
 ```
 
 # grpc安装
 #### protobuf 安装
 ```c++
$ git clone https://github.com/google/protobuf.git
$ cd protobuf
$ git submodule update --init --recursive
$ ./autogen.sh
$ ./configure
$ make
$ make check
$ sudo make install
$ sudo ldconfig # refresh shared library cache.
 ```
 #### grpc安装
 ```c++
 $ git clone -b $(curl -L https://grpc.io/release) https://github.com/grpc/grpc
 $ cd grpc
 $ git submodule update --init
 $ make
 $ [sudo] make install
 ```
 
 ### 网络配置
 ```
 root@foobar:~# cd /etc/netplan/
root@foobar:/etc/netplan# 
root@foobar:/etc/netplan# 
root@foobar:/etc/netplan# 
root@foobar:/etc/netplan# 
root@foobar:/etc/netplan# ll
total 12
drwxr-xr-x  2 root root 4096 Jun 10 07:15 ./
drwxr-xr-x 94 root root 4096 Jun 10 07:24 ../
-rw-r--r--  1 root root  257 Jun 10 07:15 00-installer-config.yaml
root@foobar:/etc/netplan# 
root@foobar:/etc/netplan# 
root@foobar:/etc/netplan# cat *
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.168.199.161/24]
      optional: true
      gateway4: 192.168.199.1
      nameservers:
              addresses: [8.8.8.8]
  version: 2
root@foobar:/etc/netplan# 
root@foobar:/etc/netplan# 
root@foobar:/etc/netplan# ip add
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:0c:29:af:31:21 brd ff:ff:ff:ff:ff:ff
    inet 192.168.199.161/24 brd 192.168.199.255 scope global ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:feaf:3121/64 scope link 
       valid_lft forever preferred_lft forever
root@foobar:/etc/netplan# 
 ```
