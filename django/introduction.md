### 快速安装指南

在你使用Django之前，你需要安装它。 我们有一个完整的安装指南，涵盖所有的可能性; 本指南将引导您进行一个简单的，最小化的安装，这将在您通读介绍时起作用。

### 安装Python

作为一个Python Web框架，Django需要Python。 请参阅我可以在Django中使用哪些Python版本？ 了解详情。 Python包含一个名为SQLite的轻量级数据库，因此您不需要设置数据库。

请通过https://www.python.org/downloads/或操作系统的软件包管理器获取最新版本的Python。


### mariadb安装
```
https://mariadb.com/下载
 wget https://downloads.mariadb.com/MariaDB/mariadb-10.2.11/repo/ubuntu/mariadb-10.2.11-ubuntu-xenial-amd64-debs.tar
```
https://mariadb.com/products/get-started

```
 pip install Django
 pip install uwsgi
 apt-get install nginx

```
#### nginx uwsgi django
http://www.linuxidc.com/Linux/2017-03/141785.htm

```
django-admin startproject mytest
python manage.py migrate
python manage.py runserver 0.0.0.0:8000
```
在root用户下启动uwsgi
```
uwsgi --ini /home/onedit/destiny/uwsgi.ini
```
uwsgi.ini:
```
[uwsgi]
socket = 127.0.0.1:9090
chdir=/home/onedit/destiny
module=wsgi
master = true
processes=2
threads=2
max-requests=2000
chmod-socket=664
vacuum=true
daemonize = /home/onedit/destiny/uwsgi.log
```

要注意：
uwsgi.ini配置文件中
module=wsgi
这里的wsgi必须是/destiny/destiny/wsgi.py，并且要把它copy到 /destiny目录下，即和uwsgi.ini在一个目录。

/etc/nginx/nginx.conf的配置：
以下配置需要放在  http{}里面。
```
upstream django {
    # server unix:///path/to/your/mysite/mysite.sock; # for a file socket
    server 127.0.0.1:9090; # for a web port socket (we'll use this first)
}
server {
    listen 80;
    server_name 47.93.37.54;
    charset     utf-8;
    access_log      /home/onedit/destiny/nginx_access.log;
    error_log       /home/onedit/destiny/nginx_error.log;
    client_max_body_size 75M;


    location /static {
        alias /home/onedit/destiny/destiny/static;
    }

    location / {
        include     /etc/nginx/uwsgi_params;
        uwsgi_pass  django;
    }
}
```
