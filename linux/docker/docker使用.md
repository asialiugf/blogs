
## 【docker-ce使用】
* https://docs.docker.com/get-started/part2/#recap-and-cheat-sheet-optional
#### breif
```
[root@dev ~]# docker --version
[root@dev ~]# docker info
[root@dev ~]# docker image ls 
```
#### detail
```
[root@dev ~]# 
[root@dev ~]# docker version
Client:
 Version:           18.06.1-ce
 API version:       1.38
 Go version:        go1.10.3
 Git commit:        e68fc7a
 Built:             Tue Aug 21 17:23:03 2018
 OS/Arch:           linux/amd64
 Experimental:      false

Server:
 Engine:
  Version:          18.06.1-ce
  API version:      1.38 (minimum version 1.12)
  Go version:       go1.10.3
  Git commit:       e68fc7a
  Built:            Tue Aug 21 17:25:29 2018
  OS/Arch:          linux/amd64
  Experimental:     false
[root@dev ~]# 
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
#### 删除容器 # docker rm 9ae111d681c4
#### 删除image # docker rmi 4ab4c602aa5e
```
[root@dev /var/lib/docker]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
friendlyhello       latest              c85d9dcb9b0d        About an hour ago   132MB
hello-world         latest              4ab4c602aa5e        11 days ago         1.84kB
python              2.7-slim            c9cde4658340        2 weeks ago         120MB
[root@dev /var/lib/docker]# 
[root@dev /var/lib/docker]# 
[root@dev /var/lib/docker]# docker rm 4ab4c602aa5e
Error: No such container: 4ab4c602aa5e
[root@dev /var/lib/docker]# 
[root@dev /var/lib/docker]# docker rmi 4ab4c602aa5e
Error response from daemon: conflict: unable to delete 4ab4c602aa5e (must be forced) - image is being used by stopped container ca6890292851
[root@dev /var/lib/docker]#
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
## 【创建image并运行】

在pyenv1目录下建议三个文件
```js
[root@dev ~/docker/pyenv1]# ll
total 12
-rw-r--r--. 1 root root 665 Sep 19 14:42 app.py
-rw-r--r--. 1 root root 512 Sep 19 14:40 Dockerfile
-rw-r--r--. 1 root root  12 Sep 19 14:42 requirements.txt
[root@dev ~/docker/pyenv1]# 
```
#### Dockerfile
```python
[root@dev ~/docker/pyenv1]# cat Dockerfile 
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
[root@dev ~/docker/pyenv1]# 
```

#### requirements.txt 
```
[root@dev ~/docker/pyenv1]# cat requirements.txt 
Flask
Redis
[root@dev ~/docker/pyenv1]# 
```

#### app.py
```python
[root@dev ~/docker/pyenv1]# cat app.py
from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>" \
           "<b>Hostname:</b> {hostname}<br/>" \
           "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
[root@dev ~/docker/pyenv1]# 
```

#### docker build -t friendlyhello .
注意后面有个点。
```
[root@dev ~/docker/pyenv1]docker build -t friendlyhello .
```
#### docker image ls 
```
[root@dev ~/docker/pyenv1]# docker image ls 
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
friendlyhello       latest              c85d9dcb9b0d        13 minutes ago      132MB
hello-world         latest              4ab4c602aa5e        11 days ago         1.84kB
python              2.7-slim            c9cde4658340        2 weeks ago         120MB
[root@dev ~/docker/pyenv1]# 
```
#### 修改 /etc/docker/daemon.json
```
[root@dev ~/docker/pyenv1]# cat /etc/docker/daemon.json
{
  "dns": ["8.8.8.8"]
}
[root@dev ~/docker/pyenv1]# 
```

#### dockerd 重启，运行friendlyhello 这个image
```
[root@dev ~/docker/pyenv1]# service docker restart
[root@dev ~/docker/pyenv1]# docker run -p 4000:80 friendlyhello
```
#### correct URL http://localhost:4000.

#### 后台运行
Now let’s run the app in the background, in detached mode:
```
# docker run -d -p 4000:80 friendlyhello
```
#### docker维护

```
[root@dev ~/docker]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
e7e8d99c4a91        friendlyhello       "python app.py"     3 minutes ago       Up 3 minutes        0.0.0.0:4000->80/tcp   hardcore_clarke
[root@dev ~/docker]# 
[root@dev ~/docker]# 
[root@dev ~/docker]# 
[root@dev ~/docker]# docker container stop e7e8d99c4a91

e7e8d99c4a91
[root@dev ~/docker]# 
[root@dev ~/docker]# 
[root@dev ~/docker]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@dev ~/docker]# 
[root@dev ~/docker]#  docker container ls --all
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                         PORTS               NAMES
e7e8d99c4a91        friendlyhello       "python app.py"     4 minutes ago       Exited (137) 9 seconds ago                         hardcore_clarke
6f7bf9f84c52        friendlyhello       "python app.py"     17 minutes ago      Exited (0) 10 minutes ago                          practical_shirley
9ae111d681c4        hello-world         "/hello"            38 minutes ago      Exited (0) 38 minutes ago                          suspicious_kepler
ca6890292851        hello-world         "/hello"            38 minutes ago      Exited (0) 38 minutes ago                          happy_davinci
2dc4128702ef        hello-world         "/hello"            About an hour ago   Exited (0) About an hour ago                       silly_fermat
879b2c3153f8        hello-world         "/hello"            16 hours ago        Exited (0) 16 hours ago                            eager_lovelace
6aef6c8a7d7c        hello-world         "/hello"            16 hours ago        Exited (0) 16 hours ago                            wonderful_stonebraker
[root@dev ~/docker]#
```
#### 重启容器 [root@dev /var/lib/docker]# docker container start 6f7bf9f84c52
#### 停止容器 [root@dev /var/lib/docker]# docker container stop 6f7bf9f84c52 
#### 重启容器 [root@dev /var/lib/docker]# docker start 6f7bf9f84c52
#### 停止容器 [root@dev /var/lib/docker]# docker stop 6f7bf9f84c52 


#### /usr/lib/systemd/system/docker.service
```
[root@dev ~/docker]# cat /usr/lib/systemd/system/docker.service 
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
#TasksMax=infinity
TimeoutStartSec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
# kill only the docker process, not all processes in the cgroup
KillMode=process
# restart the docker process if it exits prematurely
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target
[root@dev ~/docker]# 
```

#### docker 操作基本命令
Here is a list of the basic Docker commands from this page, and some related ones if you’d like to explore a bit before moving on.
```
docker build -t friendlyhello .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyhello  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyhello         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
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
