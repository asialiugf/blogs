### 官网
* https://docs.docker.com/install/linux/docker-ce/centos/#next-steps
```c++
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
                  
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

$ sudo yum-config-manager --enable docker-ce-edge

$ sudo yum-config-manager --enable docker-ce-test

$ sudo yum install docker-ce

[root@dev ~]# yum list docker-ce --showduplicates | sort -r
Installed Packages
docker-ce.x86_64         2:18.09.0.ce-1.1.beta1.el7             docker-ce-test  
docker-ce.x86_64         2:18.09.0.ce-1.1.beta1.el7             @docker-ce-test 
docker-ce.x86_64         2:18.09.0.ce-0.6.tp6.el7               docker-ce-test  
docker-ce.x86_64         2:18.09.0.ce-0.5.tp5.el7               docker-ce-test  
docker-ce.x86_64         2:18.09.0.ce-0.4.tp4.el7               docker-ce-test  
docker-ce.x86_64         2:18.09.0.ce-0.3.tp3.el7               docker-ce-test  


$ sudo systemctl enable docker.service 
$ sudo systemctl start docker

$ sudo docker run hello-world

查看日志
$ journalctl -u docker.service

```

### 终于安装成功了
可能的原因：
* 1  /var/lib/docker 这个目录删除了
* 2  执行了 yum makecache fast 这个运作。  
```
 1087  yum install docker-ce
 1088  yum remove docker-ce
 1089  yum install docker-ce
 1090  yum install docker
 1091  yum remove docker
 1092  yum install docker
 1093  which docker
 1094   yum remove docker-ce
 1095  yum install docker
 1096  yum remove docker docker-common container-selinux docker-selinux docker-engine
 1097  y
 1098  yum remove docker docker-common container-selinux docker-selinux docker-engine
 1099  yum install -y yum-utils
 1100  yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
 1101   yum makecache fast
 1102   yum install docker-ce
 1103  yum remore docker-ce-cli
 1104  yum remove docker-ce-cli
 1105   yum install docker-ce
 1106  which docker
 1107  systemctl start docker
 1108  ps -ef | grep docker
 1109  docker run hello-world
 1110  history
[root@dev ~]# 
```
