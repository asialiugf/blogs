## nginx install 20180920
```
/bin/ld: cannot find -lstdc++
collect2: error: ld returned 1 exit status
make[1]: *** [libpcrecpp.la] Error 1
make[1]: Leaving directory `/root/source/pcre-8.42'
make: *** [all] Error 2
[root@iZ23psatkqsZ ~/source/pcre-8.42]# 
[root@iZ23psatkqsZ ~/source/pcre-8.42]# yum install glibc-static libstdc++-static
```
```
[root@iZ23psatkqsZ ~/source/pcre-8.42]# wget https://ftp.pcre.org/pub/pcre/pcre-8.42.tar.gz
tar xvf pcre-8.42.tar.gz
cd pcre-8.42    
./configure
make
make install
```

* https://github.com/nginx/nginx/releases
```
[root@iZ23psatkqsZ ~/source]# wget https://github.com/nginx/nginx/archive/release-1.15.3.tar.gz
tar xvf release-1.15.3.tar.gz
cd nginx*
cp ./auto/configure .
./configure
make
make install
```


***

## nginx install

#### 先安装 prce
https://ftp.pcre.org/pub/pcre/

```c
wget https://ftp.pcre.org/pub/pcre/pcre-8.00.tar.bz2
tar xvf pcre-8.00.tar.bz2
cd pcre-8.00
./configure
make
make install
```

#### 在以下网站下载最新nginx

https://www.nginx.com/resources/wiki/start/topics/tutorials/install/

wget http://nginx.org/download/nginx-1.10.1.tar.gz?_ga=2.197258858.430340656.1514522819-537622954.1514522819
```c
tar xvf nginx-1.10.1.tar.gz
cd nginx-1.10.1
./configure
make
make install
```
#### 安装目录：
```c
root@d:/usr/local/nginx# ll
total 16
drwxr-xr-x 2 root root 4096 Dec 29 13:06 conf
drwxr-xr-x 2 root root 4096 Dec 29 13:06 html
drwxr-xr-x 2 root root 4096 Dec 29 13:06 logs
drwxr-xr-x 2 root root 4096 Dec 29 13:06 sbin
root@d:/usr/local/nginx# 
```
#### nginx的配置文件 /usr/local/nginx/conf下面
```c
root@d:/usr/local/nginx/conf# cat nginx.conf

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    upstream websocket {
        server 147.93.57.54:3000;
    }

    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }

    server {
        listen       80;
        server_name  147.93.57.54;

        location /api/push/ws {
            proxy_pass http://websocket;

            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
        }


        location / {
            root  /usr/local/nginx/html;
            index  index.html index.htm;
        }

        location ~ .*\.(html|htm|gif|jpg|jpeg|bmp|png|ico|txt|js|css)$
        {
            #root /usr/local/nginx/html/static;
            root /home/riddle/uquant/html;
            #expires      7d; 
        }




        #charset koi8-r;
        #access_log  logs/host.access.log  main;
        #error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
root@d:/usr/local/nginx/conf# 
```

