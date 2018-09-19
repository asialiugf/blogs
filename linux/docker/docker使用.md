## 【docker-ce使用】
***




### 进入 docker
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
