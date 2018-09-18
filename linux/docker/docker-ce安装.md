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

$ sudo systemctl start docker

$ sudo docker run hello-world


```
