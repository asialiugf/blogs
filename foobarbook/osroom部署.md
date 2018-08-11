### 
* https://osroom.com/doc/v1.0/app/deploy/

部署方式
  Nginx+uwsgi
部署Python Web的方式有好多中, 这里只是举例其中一种方式, 使用Nginx + uwsgi部署. 查阅资料,认为uwsgi性能还不错 


uwsgi安装与配置
  安装
pip 安装uwsgi(如果有建有python虚拟环境的请先进入虚拟环境再安装)
pip install uwsgi



  配置
新建一个文件uwsgi.ini (建议在osroom根目录下)，写入如下配置:
[uwsgi]
# 使用uwsgi示范
# uwsgi 启动时所使用的地址与端口
socket = 127.0.0.1:6001
# 指向网站目录
chdir=/home/work/project/osroom
# python 启动程序文件
wsgi-file = start.py

# python 程序内用以启动的 application 变量名
callable = app
master = true
enable-threads = true

# 启动的线程
processes = 4
vacuum = true
die-on-term = true
harakiri = 30

# 每一个工作进程都生成以后才加载应用程序
lazy = true
disable-logging = true
# 状态检测地址
stats = 127.0.0.1:9191
# pid
pidfile = /tmp/osroom_uwsgi.pid

其他配置项可以参考uwsgi文档 


  启动网站
注意：启动前确认osroom的数据库是否已配置正确
启动命令
# 如果uwsgi装在python虚拟环境，要先进入虚拟环境,再用如下命令启动
uwsgi /home/work/project/osroom/uwsgi.ini


（附加）方法2：如果uwsgi安装在python虚拟环境中，又不想进入虚拟环境，可以这样启动

# venv_osroom是我的虚拟环境目录
/home/work/project/venv_osroom/bin/uwsgi /home/work/project/osroom/uwsgi.ini


- 启动成功后: 由于uwsgi.ini配置的端口是6001，所以不能通过其他端口访问网站，需要下面配置Nginx转发到6001端口

...

uwsgi socket 0 bound to TCP address 127.0.0.1:6001 fd 3
Python version: 3.5.2 (default, Nov 23 2017, 16:37:01)  [GCC 5.4.0 20160609]
Python main interpreter initialized at 0x1b9bcd0
python threads support enabled

...

*** Operational MODE: preforking ***
*** uWSGI is running in multiple interpreter mode ***
spawned uWSGI master process (pid: 22449)
spawned uWSGI worker 1 (pid: 22450, cores: 1)
spawned uWSGI worker 2 (pid: 22451, cores: 1)
spawned uWSGI worker 3 (pid: 22452, cores: 1)
spawned uWSGI worker 4 (pid: 22453, cores: 1)

...

worker 1 buried after 1 seconds
worker 2 buried after 1 seconds
worker 3 buried after 1 seconds
worker 4 buried after 1 seconds

...




Nginx 安装与配置
  安装
Ubuntu 14.04或Ubuntu 16.04使用apt-get直接安装如下：
sudo apt-get install nginx



也可以使用wget下载，再安装，请自行Google查找教程哈
  配置文件



创建一个nginx配置文件osroom-naginx.conf，文件配置如下(这里只是示范，Nginx有许多配置项可以自己查找教程):
upstream osroom-web {
      # 转发到6001端口
      server 127.0.0.1:6001;
}

server {
        # 监听80端端口
        listen 80;
        server_name <你的域名>;
        gzip on;
        gzip_comp_level 5;
        gzip_types application/json text/plain application/javascript application/x-javascript text/javascript text/xml text/css;
        open_log_file_cache max=1000 inactive=20s valid=1m min_uses=2;
        access_log /var/log/nginx/manage.vhost.access.log;
        error_log /var/log/nginx/manage.vhost.error.log;
        location / {
             include      uwsgi_params;
             # upstream 的那个名称
             uwsgi_pass osroom-web;
             # python虚拟环境目录路径 
             uwsgi_param UWSGI:_PYHOME /home/work/project/venv_osroom;
             # 项目目录路径 
             uwsgi_param UWSGI_CHDIR /home/work/project/osroom-demo;;
             uwsgi_param UWSGI_SCRIPT start:app;
          }
}




如果需要配置ssl请加上监听443端口，配置好证书，然后80端口301重定向到443 https请求, 如下: (不需配置SSL证书的请直接跳过此步骤)
upstream osroom-web {
      # 转发到6001端口
      server 127.0.0.1:6001;
}

server {
        # 监听443端端口
        listen 443;
        server_name <你的域名>;
        ssl on;
        root html;
        index index.html index.htm;
        # ssl证书文件
        ssl_certificate   /home/work/project/15212232323.pem;
        ssl_certificate_key  /home/work/project/15212232323.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        gzip on;
        gzip_comp_level 5;
        gzip_types application/json text/plain application/javascript application/x-javascript text/javascript text/xml text/css;
        open_log_file_cache max=1000 inactive=20s valid=1m min_uses=2;
        access_log /var/log/nginx/manage.vhost.access.log;
        error_log /var/log/nginx/manage.vhost.error.log;
        location / {
             include      uwsgi_params;
             # upstream 的那个名称
             uwsgi_pass osroom-web;
             # python虚拟环境目录路径 
             uwsgi_param UWSGI:_PYHOME /home/work/project/venv_osroom;
             # 项目目录路径 
             uwsgi_param UWSGI_CHDIR /home/work/project/osroom-demo;;
             uwsgi_param UWSGI_SCRIPT start:app;
          }
}

server {
        listen 80;
        server_name <你的域名>;
        return    301 https://$server_name$request_uri;
}



写好配置文件后编辑nginx主配置文件include 上一步写的配置文件
# Ubuntu apt-get安装后的Nginx配置文件在/etc/nginx/nginx.conf
# 编辑/etc/nginx/nginx.conf找到在http {}里面最后添加osroom.conf 
# 可以用*通配符，前提是该目录下没有其他非nginx得.conf配置文件
http {

   ...

  include /home/work/project/*.conf;
}




  重启Nginx
sudo /etc/init.d/nginx restart



  访问
这个时候就可以通过域名（域名有解析到服务器的情况下）或IP访问OSROOM了
