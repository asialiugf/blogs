#### 修改主机名
```
[root@bogon ~]# hostnamectl set-hostname  my_name
reboot
不需要reboot，重新登录即可。
```
#### 增加硬盘
```
cd /dev
ls /dev/vdb*
[root@i-3913FACC dev]# fdisk /dev/vdb
[root@i-3913FACC dev]# mkfs -t ext4 -c /dev/vdb1
[root@i-3913FACC /]# mkdir /disk0
[root@i-3913FACC /]# mount /dev/vdb1 /disk0
```

```
[root@dev_fileserver ~]#  blkid

/dev/vda1: UUID="83812942-25e3-4ef9-b8a1-3b315af97921" TYPE="xfs" 
/dev/vda2: UUID="38MpAa-nAnq-xMOW-Dv43-w4Th-mpnu-d9VQW3" TYPE="LVM2_member" 
/dev/vdb1: UUID="60de70f8-dfd1-41d2-9127-e8a35c93e1a5" TYPE="ext4" 
/dev/mapper/centos-root: UUID="f86be81a-4a09-481d-a09b-73aeff018d07" TYPE="xfs" 
/dev/mapper/centos-swap: UUID="e5e41ca8-77b6-42ed-a984-ce50191f5b7c" TYPE="swap" 
[root@dev_fileserver ~]# 
```
#### 在/etc/fstab文件中加入最后一行
* UUID=60de70f8-dfd1-41d2-9127-e8a35c93e1a5 /disk0                 ext4     defaults        0 0
```
[root@dev_fileserver ~]# cat /etc/fstab

#
# /etc/fstab
# Created by anaconda on Thu Aug 23 00:26:06 2018
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=83812942-25e3-4ef9-b8a1-3b315af97921 /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0
UUID=60de70f8-dfd1-41d2-9127-e8a35c93e1a5 /disk0                 ext4     defaults        0 0
[root@dev_fileserver ~]# 
```
* 重启机器
```
# reboot
```


```
[root@i-3913FACC /]# df -k
æ–‡ä»¶ç³»ç»Ÿ                   1K-å—    å·²ç”¨     å¯ç”¨ å·²ç”¨% æŒ‚è½½ç‚¹
/dev/mapper/centos-root 30560640 1130228 29430412    4% /
devtmpfs                  496640       0   496640    0% /dev
tmpfs                     508144       0   508144    0% /dev/shm
tmpfs                     508144    6848   501296    2% /run
tmpfs                     508144       0   508144    0% /sys/fs/cgroup
/dev/vda1                1038336  128296   910040   13% /boot
tmpfs                     101632       0   101632    0% /run/user/0
tmpfs                     101632       0   101632    0% /run/user/1001
/dev/vdb1               51473888   53272 48782844    1% /disk0
[root@i-3913FACC /]# 
```

#### yum install gcc gcc-c++
#### astyle 安装
```c++
[root@iZ23psatkqsZ mysql]# wget https://nchc.dl.sourceforge.net/project/astyle/astyle/astyle%203.0.1/astyle_3.0.1_linux.tar.gz
tar xvf *.gz
cd /root/github/astyle/build/gcc
# make
# make install
```
#### bzip2
* yum install -y bzip2


#### 安装软件新方法
* https://blog.csdn.net/supergao222/article/details/78308197
