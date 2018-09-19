
## 【docker-ce使用】

#### breif
```
[root@dev ~]# docker --version
[root@dev ~]# docker info
[root@dev ~]# docker image ls 
```
#### detail
```
[root@dev ~]# docker --version
Docker version 18.06.1-ce, build e68fc7a
[root@dev ~]# 
```
#### docker ps -a
```
[root@dev ~]# docker ps -a          
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
9ae111d681c4        hello-world         "/hello"            4 seconds ago       Exited (0) 2 seconds ago                        suspicious_kepler
ca6890292851        hello-world         "/hello"            29 seconds ago      Exited (0) 26 seconds ago                       happy_davinci
2dc4128702ef        hello-world         "/hello"            43 minutes ago      Exited (0) 43 minutes ago                       silly_fermat
879b2c3153f8        hello-world         "/hello"            15 hours ago        Exited (0) 15 hours ago                         eager_lovelace
6aef6c8a7d7c        hello-world         "/hello"            15 hours ago        Exited (0) 15 hours ago                         wonderful_stonebraker
[root@dev ~]#
```

#### docker container ls --all
```
[root@dev ~]# docker container ls --all
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                         PORTS               NAMES
9ae111d681c4        hello-world         "/hello"            3 minutes ago       Exited (0) 3 minutes ago                           suspicious_kepler
ca6890292851        hello-world         "/hello"            3 minutes ago       Exited (0) 3 minutes ago                           happy_davinci
2dc4128702ef        hello-world         "/hello"            About an hour ago   Exited (0) About an hour ago                       silly_fermat
879b2c3153f8        hello-world         "/hello"            15 hours ago        Exited (0) 15 hours ago                            eager_lovelace
6aef6c8a7d7c        hello-world         "/hello"            15 hours ago        Exited (0) 15 hours ago                            wonderful_stonebraker
[root@dev ~]# 
```

#### docker images
```
[root@dev ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              4ab4c602aa5e        11 days ago         1.84kB
[root@dev ~]# 
```
#### docker image ls 
```
[root@dev ~/docker/pyenv1]# docker image ls 
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
friendlyhello       latest              c85d9dcb9b0d        About a minute ago   132MB
hello-world         latest              4ab4c602aa5e        11 days ago          1.84kB
python              2.7-slim            c9cde4658340        2 weeks ago          120MB
[root@dev ~/docker/pyenv1]#
```



***

## 【进入 docker】
```
[root@dev ~]# docker ps -a
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                  NAMES
16fcd3d6e6e8        nginx                   "nginx -g 'daemon ..."   About an hour ago   Up 8 minutes        0.0.0.0:6800->80/tcp   nginx-health-web-pc
869a00ade7f4        kibana:5.2.0-2          "/docker-entrypoin..."   4 months ago        Created                                    l_kan.1.gq9b7x5kvxa3x426xarmbbe33
a53097012e02        elasticsearch:5.2.0-2   "/docker-entrypoin..."   4 months ago        Created                                    l_es.1.svf8q1ydxfwrhlv48ooeka3h4
ac28e7434410        dns_server:v3           "/bin/sh -c /usr/b..."   4 months ago        Created                                    dns.1.wlhhteurofxm405vq2qgcs1ej
[root@dev ~]# 
[root@dev ~]# docker exec -it 16fcd3d6e6e8 /bin/bash
root@16fcd3d6e6e8:/# 
```
### 重启
```
[root@dev ~]# docker restart nginx-health-web-pc
```
### Docker部署nginx并修改配置文件
* https://blog.csdn.net/wangfei0904306/article/details/77623400
