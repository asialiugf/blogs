### 【一】环境
```python
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


### 【三】 
