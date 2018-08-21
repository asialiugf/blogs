### 【一】环境
```c
python version !!!!
[gethtml@iZ23psatkqsZ ~]$ python --version
Python 3.6.5 :: Anaconda, Inc.
[gethtml@iZ23psatkqsZ ~]$ 

[gethtml@iZ23psatkqsZ ~]$ which python
/root/anaconda3/bin/python
[gethtml@iZ23psatkqsZ ~]$ 

django version !!!!
[gethtml@iZ23psatkqsZ ~]$ python -m django --version
2.1
[gethtml@iZ23psatkqsZ ~]$ 

[gethtml@iZ23psatkqsZ ~]$ which django-admin
/root/anaconda3/bin/django-admin
[gethtml@iZ23psatkqsZ ~]$ 
```

### 【二】 create project!!
```
[gethtml@iZ23psatkqsZ ~]$ django-admin startproject foobar
[gethtml@iZ23psatkqsZ ~]$ cd foobar/
[gethtml@iZ23psatkqsZ ~/foobar]$ python manage.py migrate
[gethtml@iZ23psatkqsZ ~/foobar]$ python manage.py runserver 0.0.0.0:8000
```
```
[gethtml@iZ23psatkqsZ ~]$ django-admin startproject foobar
[gethtml@iZ23psatkqsZ ~]$ tree foobar
foobar
├── foobar
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py

1 directory, 5 files
[gethtml@iZ23psatkqsZ ~]$ 
```
```
[gethtml@iZ23psatkqsZ ~]$ cd foobar/
[gethtml@iZ23psatkqsZ ~/foobar]$ ll
total 8
drwxrwxr-x 2 gethtml gethtml 4096 Aug 21 23:31 foobar
-rwxr-xr-x 1 gethtml gethtml  538 Aug 21 23:31 manage.py
[gethtml@iZ23psatkqsZ ~/foobar]$ 
[gethtml@iZ23psatkqsZ ~/foobar]$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying sessions.0001_initial... OK
[gethtml@iZ23psatkqsZ ~/foobar]$ 
```
```
[gethtml@iZ23psatkqsZ ~/foobar]$ python manage.py runserver 0.0.0.0:8000
Performing system checks...

System check identified no issues (0 silenced).
August 21, 2018 - 15:35:47
Django version 2.1, using settings 'foobar.settings'
Starting development server at http://0.0.0.0:8000/
Quit the server with CONTROL-C.
```
浏览器打开显示
```
DisallowedHost at /
Invalid HTTP_HOST header: '121.199.28.62:8000'. You may need to add '121.199.28.62' to ALLOWED_HOSTS.
Request Method:	GET
Request URL:	http://121.199.28.62:8000/
Django Version:	2.1
Exception Type:	DisallowedHost
Exception Value:	
Invalid HTTP_HOST header: '121.199.28.62:8000'. You may need to add '121.199.28.62' to ALLOWED_HOSTS.
Exception Location:	/root/anaconda3/lib/python3.6/site-packages/django/http/request.py in get_host, line 106
Python Executable:	/root/anaconda3/bin/python
Python Version:	3.6.5
Python Path:	
['/home/gethtml/foobar',
 '/root/anaconda3/lib/python36.zip',
 '/root/anaconda3/lib/python3.6',
 '/root/anaconda3/lib/python3.6/lib-dynload',
 '/root/anaconda3/lib/python3.6/site-packages']
Server time:	Tue, 21 Aug 2018 15:36:25 +0000
```
#### 修改 ~/foobar/foobar/setting.py
```
[gethtml@iZ23psatkqsZ ~/foobar/foobar]$ pwd
/home/gethtml/foobar/foobar
[gethtml@iZ23psatkqsZ ~/foobar/foobar]$ ll
total 16
-rw-r--r-- 1 gethtml gethtml    0 Aug 21 23:31 __init__.py
drwxrwxr-x 2 gethtml gethtml 4096 Aug 21 23:35 __pycache__
-rw-r--r-- 1 gethtml gethtml 3089 Aug 21 23:39 settings.py
-rw-r--r-- 1 gethtml gethtml  748 Aug 21 23:31 urls.py
-rw-r--r-- 1 gethtml gethtml  389 Aug 21 23:31 wsgi.py
[gethtml@iZ23psatkqsZ ~/foobar/foobar]$
```

####　setting.py的配置中加上以下：
```
ALLOWED_HOSTS = ["*"]
STATIC_ROOT = '/home/gethtml/foobar'
```

### 【三】 nginx 配置
 /etc/nginx/nginx.conf
```
    server {
        listen 8996; #暴露给外部访问的端口
        server_name localhost;
            charset utf-8;
        location / {
            include uwsgi_params;
            uwsgi_pass 127.0.0.1:8997; #外部访问8996就转发到内部8997
        }
        location /static/ {
            # alias /home/gethtml/mytest/; #项目静态路径设置
            alias /home/gethtml/foobar/; #项目静态路径设置
        }
    }
```
### 【四】 uwsgi 配置
```
[gethtml@iZ23psatkqsZ ~/foobar]$ cat uwsgi.ini
[uwsgi]
socket = 127.0.0.1:8997
chdir=/home/gethtml/foobar
module=foobar.wsgi
master = true
processes=2
threads=2
max-requests=2000
chmod-socket=664
vacuum=true
daemonize = /home/gethtml/foobar/uwsgi.log
[gethtml@iZ23psatkqsZ ~/foobar]$ 
```
```
ps -ef | grep uwsgi | grep -v grep | awk '{print $2}' | xargs kill -9
```
#### 修改了 urls.py 要重启 uwsgi

### 【五】 启动

* 启动 uwsgi
```
[gethtml@iZ23psatkqsZ ~/foobar]$ uwsgi --ini uwsgi.ini                     
[uWSGI] getting INI configuration from uwsgi.ini
[gethtml@iZ23psatkqsZ ~/foobar]$ 
or
[gethtml@iZ23psatkqsZ ~/foobar]$ uwsgi --ini /home/gethtml/foobar/uwsgi.ini  
```
* 启动 nginx ( root )
```
[root@iZ23psatkqsZ /etc/nginx]# nginx -s stop
[root@iZ23psatkqsZ /etc/nginx]# nginx
[root@iZ23psatkqsZ /etc/nginx]# which nginx
/sbin/nginx
[root@iZ23psatkqsZ /etc/nginx]# 
```
### 【六】 访问
* http://121.199.28.62:8996/admin
* http://121.199.28.62:8996

### 【七】 修改static (要和前面的nginx的配置一致）
####　~/foobar/foobar/setting.py的配置中加上以下：
```
ALLOWED_HOSTS = ["*"]
# 在最后加：
STATIC_URL = '/static/'
STATIC_ROOT = '/home/gethtml/foobar'
```
然后执行以下命令：
```
[gethtml@iZ23psatkqsZ ~/foobar]$ python manage.py collectstatic

You have requested to collect static files at the destination
location as specified in your settings:

    /home/gethtml/foobar

This will overwrite existing files!
Are you sure you want to do this?

Type 'yes' to continue, or 'no' to cancel: yes

119 static files copied to '/home/gethtml/foobar'.
[gethtml@iZ23psatkqsZ ~/foobar]$ 
[gethtml@iZ23psatkqsZ ~/foobar]$ ll
total 148
drwxrwxr-x 6 gethtml gethtml   4096 Aug 22 00:00 admin
-rw-r--r-- 1 gethtml gethtml 131072 Aug 21 23:34 db.sqlite3
drwxrwxr-x 3 gethtml gethtml   4096 Aug 21 23:57 foobar
-rwxr-xr-x 1 gethtml gethtml    538 Aug 21 23:31 manage.py
-rw-rw-r-- 1 gethtml gethtml    204 Aug 21 23:47 uwsgi.ini
-rw-r----- 1 gethtml gethtml   3036 Aug 21 23:52 uwsgi.log
[gethtml@iZ23psatkqsZ ~/foobar]$ cd admin
[gethtml@iZ23psatkqsZ ~/foobar/admin]$ ll
total 16
drwxrwxr-x 3 gethtml gethtml 4096 Aug 22 00:00 css
drwxrwxr-x 2 gethtml gethtml 4096 Aug 22 00:00 fonts
drwxrwxr-x 3 gethtml gethtml 4096 Aug 22 00:00 img
drwxrwxr-x 4 gethtml gethtml 4096 Aug 22 00:00 js
[gethtml@iZ23psatkqsZ ~/foobar/admin]$ cd -
/home/gethtml/foobar
[gethtml@iZ23psatkqsZ ~/foobar]$ 
```
### 【七】 添加管理用户
```
[gethtml@iZ23psatkqsZ ~/foobar]$ python manage.py createsuperuser
Username (leave blank to use 'gethtml'): admin
Email address: a@a.co
Password: 
Password (again): 
This password is too short. It must contain at least 8 characters.
This password is too common.
This password is entirely numeric.
Bypass password validation and create user anyway? [y/N]: 
Password: 
Password (again): 
This password is too short. It must contain at least 8 characters.
This password is too common.
This password is entirely numeric.
Bypass password validation and create user anyway? [y/N]: y
Superuser created successfully.
[gethtml@iZ23psatkqsZ ~/foobar]$ 
```
这样就可以在 http://121.199.28.62:8996/admin 后台中登录了。






