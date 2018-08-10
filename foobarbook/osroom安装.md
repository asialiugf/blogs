### 下载osroom
* https://github.com/osroom/osroom
```c
git clone https://github.com/osroom/osroom.git
```
```c++
# cd osroom/apps
/root/github/osroom/apps/configs
[root@iZ23psatkqsZ configs]# 
[root@iZ23psatkqsZ configs]# ll
total 92
-rw-r--r-- 1 root   root   30526 Aug 10 23:33 config.py
-rw-r--r-- 1 197608 197121 31394 Aug 10 02:27 config_sample.py
-rw-r--r-- 1 root   root    1232 Aug 10 23:28 db_config.py
-rw-r--r-- 1 197608 197121  1253 Aug 10 02:27 db_config_sample.py
-rw-r--r-- 1 197608 197121    48 Aug 10 02:27 __init__.py
-rw-r--r-- 1 197608 197121  1469 Aug 10 02:27 mdb_collection.py
drwxr-xr-x 2 root   root    4096 Aug 10 23:33 __pycache__
-rwxr-xr-x 1 197608 197121  4108 Aug 10 02:27 sys_config.py
[root@iZ23psatkqsZ configs]#
```

### mongodb redis 配置
```python
[root@iZ23psatkqsZ configs]# cat db_config.py
# -*-coding:utf-8-*-
__author__ = "Allen Woo"
DB_CONFIG = {
    "mongodb": {
        "mongo_sys": {
            "username": "myadmin",
            "config": {
                "fsync": False,
                "replica_set": None
            },
            "password": "secret",
            "dbname": "osr_sys",
            "host": [
                "127.0.0.1:27017"
            ]
        },
        "mongo_user": {
            "username": "myadmin",
            "config": {
                "fsync": False,
                "replica_set": None
            },
            "password": "secret",
            "dbname": "osr_user",
            "host": [
                "127.0.0.1:27017"
            ]
        },
        "mongo_web": {
            "username": "myadmin",
            "config": {
                "fsync": False,
                "replica_set": None
            },
            "password": "secret",
            "dbname": "osr_web",
            "host": [
                "127.0.0.1:27017"
            ]
        }
    },
    "redis": {
        "password": "foobarbook",
        "port": [
            "6379"
        ],
        "host": [
            "127.0.0.1"
        ]
    }
}
[root@iZ23psatkqsZ configs]# 
```


### mongodb配置
```c++
[root@iZ23psatkqsZ osroom]# mongo
MongoDB shell version v4.0.1
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 4.0.1
Server has startup warnings: 
2018-08-10T22:16:15.935+0800 I STORAGE  [initandlisten] 
2018-08-10T22:16:15.935+0800 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2018-08-10T22:16:15.935+0800 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2018-08-10T22:16:16.942+0800 I CONTROL  [initandlisten] 
2018-08-10T22:16:16.942+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2018-08-10T22:16:16.942+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] 
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] 
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] 
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2018-08-10T22:16:16.943+0800 I CONTROL  [initandlisten] 
---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display
metrics about your deployment (disk utilization, CPU, operation statistics, etc).

The monitoring data will be available on a MongoDB website with a unique URL accessible to you
and anyone you share the URL with. MongoDB may use this information to make product
improvements and to suggest MongoDB products and deployment options to you.

To enable free monitoring, run the following command: db.enableFreeMonitoring()
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---

> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> 
> use osr_sys;
switched to db osr_sys
> db.createUser({ user: "root", pwd: "foobarbook", roles: [{ role: "userAdminAnyDatabase", db: "osr_sys" }] })
2018-08-10T23:15:16.463+0800 E QUERY    [js] Error: couldn't add user: No role named userAdminAnyDatabase@osr_sys :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
DB.prototype.createUser@src/mongo/shell/db.js:1491:15
@(shell):1:1
> 
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> 
> use osr_sys
switched to db osr_sys
>  db.createUser({ user: "root", pwd: "foobarbook");
... 
... ;
... 
... 
> 
> 
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> use osr_sys
switched to db osr_sys
> 
> db.addUser({ user: "root", pwd: "foobarbook"})
2018-08-10T23:16:55.465+0800 E QUERY    [js] TypeError: db.addUser is not a function :
@(shell):1:1
> 
> db.createUser({ user: "root", pwd: "foobarbook"})
2018-08-10T23:17:07.323+0800 E QUERY    [js] Error: couldn't add user: "createUser" command requires a "roles" array :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
DB.prototype.createUser@src/mongo/shell/db.js:1491:15
@(shell):1:1
> db.addUser({ user: "root", pwd: "foobarbook"})
2018-08-10T23:18:45.050+0800 E QUERY    [js] TypeError: db.addUser is not a function :
@(shell):1:1
> 
> 
> 
> 
> use tt
switched to db tt
> db.addUser('mongodb','123456');
2018-08-10T23:18:48.311+0800 E QUERY    [js] TypeError: db.addUser is not a function :
@(shell):1:1
> 
> 
> use osr_sys
switched to db osr_sys
> db.createUser(
...      {
...        user:"myadmin",
...        pwd:"secret",
...        roles:[{role:"root",db:"admin"}]
...      }
...   )
Successfully added user: {
        "user" : "myadmin",
        "roles" : [
                {
                        "role" : "root",
                        "db" : "admin"
                }
        ]
}
> 
> 
> 
> use osr_user
switched to db osr_user
> db.createUser(
...      {
...        user:"myadmin",
...        pwd:"secret",
...        roles:[{role:"root",db:"admin"}]
...      }
...   )
Successfully added user: {
        "user" : "myadmin",
        "roles" : [
                {
                        "role" : "root",
                        "db" : "admin"
                }
        ]
}
> show uses
2018-08-10T23:22:44.945+0800 E QUERY    [js] Error: don't know how to show [uses] :
shellHelper.show@src/mongo/shell/utils.js:1055:11
shellHelper@src/mongo/shell/utils.js:766:15
@(shellhelp2):1:1
> 
> use osr_user
switched to db osr_user
> show uses
2018-08-10T23:23:33.479+0800 E QUERY    [js] Error: don't know how to show [uses] :
shellHelper.show@src/mongo/shell/utils.js:1055:11
shellHelper@src/mongo/shell/utils.js:766:15
@(shellhelp2):1:1
> 
> show use
2018-08-10T23:23:36.636+0800 E QUERY    [js] Error: don't know how to show [use] :
shellHelper.show@src/mongo/shell/utils.js:1055:11
shellHelper@src/mongo/shell/utils.js:766:15
@(shellhelp2):1:1
> 
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> 
> 
> 
> use admin 
switched to db admin
> db.createUser(
...      {
...        user:"myadmin",
...        pwd:"secret",
...        roles:[{role:"root",db:"admin"}]
...      }
...   )
Successfully added user: {
        "user" : "myadmin",
        "roles" : [
                {
                        "role" : "root",
                        "db" : "admin"
                }
        ]
}
# # 查
2018-08-10T23:25:28.211+0800 E QUERY    [js] SyntaxError: illegal character @(shell):1:0
> 
> 
> show users
{
        "_id" : "admin.myadmin",
        "user" : "myadmin",
        "db" : "admin",
        "roles" : [
                {
                        "role" : "root",
                        "db" : "admin"
                }
        ],
        "mechanisms" : [
                "SCRAM-SHA-1",
                "SCRAM-SHA-256"
        ]
}
> 
> 
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> db.auth('myadmin', 'secret')
1
> use osr_web
switched to db osr_web
> db.createUser(
...      {
...        user:"myadmin",
...        pwd:"secret",
...        roles:[{role:"root",db:"admin"}]
...      }
...   )
# Successfully added user: {
        "user" : "myadmin",
        "roles" : [
                {
                        "role" : "root",
                        "db" : "admin"
                }
        ]
}
> # 
2018-08-10T23:28:50.084+0800 E QUERY    [js] SyntaxError: illegal character @(shell):1:0
> 
```

### osroom 初始化
```python
[root@iZ23psatkqsZ osroom]# python start.py add_user
 * Check or update Python third-party libraries
 * Update pip...
Looking in indexes: http://mirrors.aliyun.com/pypi/simple/
Requirement already up-to-date: pip in /root/anaconda3/lib/python3.6/site-packages (18.0)
 * Now don't need python library:
['ply==3.11', 'wrapt==1.10.11', 'ruamel-yaml==0.15.35', 'Bottleneck==1.2.1', 'numexpr==2.6.5', 'ptyprocess==0.5.2', 'mpmath==1.0.0', 'isort==4.3.4', 'pyOpenSSL==18.0.0', 'numpydoc==0.8.0', 'pytest-openfiles==0.3.0', 'path.py==11.0.1', 'distributed==1.21.8', 'pickleshare==0.7.4', 'pycosat==0.6.3', 'conda-verify==2.0.0', 'msgpack==0.5.6', 'msgpack-python==0.5.6', 'matplotlib==2.2.2', 'jedi==0.12.0', 'scikit-image==0.13.1', 'pluggy==0.6.0', 'llvmlite==0.23.1', 'qtconsole==4.3.1', 'typing==3.6.4', 'pytest-arraydiff==0.2', 'jupyterlab==0.32.1', 'lazy-object-proxy==1.3.1', 'heapdict==1.0.0', 'fastcache==1.0.2', 'networkx==2.1', 'pytest-doctestplus==0.1.3', 'jupyter-core==4.4.0', 'glob2==0.6', 'simplegeneric==0.8.1', 'parso==0.2.0', 'terminado==0.8.1', 'kiwisolver==1.0.1', 'boto==2.48.0', 'Cython==0.28.2', 'zict==0.1.3', 'cssselect==1.0.3', 'ipywidgets==7.2.1', 'docutils==0.14', 'anaconda-navigator==1.8.7', 'singledispatch==3.4.0.3', 'odo==0.5.1', 'jupyter-console==5.2.0', 'blaze==0.11.3', 'xlwt==1.3.0', 'clyent==1.2.2', 'Send2Trash==1.5.0', 'ipython-genutils==0.2.0', 'sortedcontainers==1.5.10', 'pyodbc==4.0.23', 'jupyter-client==5.2.3', 'navigator-updater==0.2.1', 'traitlets==4.3.2', 'statsmodels==0.9.0', 'attrs==18.1.0', 'imagesize==1.0.0', 'u-msgpack-python==2.5.0', 'contextlib2==0.5.5', 'decorator==4.3.0', 'nltk==3.3', 'prompt-toolkit==1.0.15', 'pycurl==7.43.0.1', 'nbformat==4.4.0', 'et-xmlfile==1.0.1', 'html5lib==1.0.1', 'colorama==0.3.9', 'SQLAlchemy==1.2.7', 'pandocfilters==1.4.2', 'pexpect==4.5.0', 'cytoolz==0.9.0.1', 'entrypoints==0.2.3', 'mkl-random==1.0.1', 'py==1.5.3', 'pytest-remotedata==0.2.1', 'ipykernel==4.8.2', 'pep8==1.7.1', 'scipy==1.1.0', 'webencodings==0.5.1', 'bokeh==0.12.16', 'nbconvert==5.3.1', 'backports.shutil-get-terminal-size==1.0.0', 'gmpy2==2.0.8', 'pytest==3.5.1', 'pytest-astropy==0.3.0', 'ipython==6.4.0', 'pyquery==1.4.0', 'pkginfo==1.4.2', 'sympy==1.1.1', 'packaging==17.1', 'bitarray==0.8.1', 'jsmin==2.2.2', 'toolz==0.9.0', 'mccabe==0.6.1', 'beautifulsoup4==4.6.0', 'pyzmq==17.0.0', 'jupyterlab-launcher==0.10.5', 'wcwidth==0.1.7', 'tornado==4.5.3', 'XlsxWriter==1.0.4', 'Pygments==2.2.0', 'jdcal==1.4', 'cycler==0.10.0', 'pylint==1.8.4', 'dask==0.17.5', 'greenlet==0.4.13', 'numpy==1.14.3', 'gevent==1.3.0', 'Flask-Cors==3.0.4', 'jsonschema==2.6.0', 'openpyxl==2.5.3', 'scikit-learn==0.19.1', 'multipledispatch==0.5.0', 'filelock==3.0.4', 'seaborn==0.8.1', 'patsy==0.5.0', 'pycodestyle==2.4.0', 'QtPy==1.4.1', 'rope==0.10.7', 'backcall==0.1.0', 'unicodecsv==0.14.1', 'numba==0.38.0', 'pyparsing==2.2.0', 'cloudpickle==0.5.3', 'defusedxml==0.5.0', 'more-itertools==4.1.0', 'spyder==3.2.8', 'astroid==1.6.3', 'sortedcollections==0.6.1', 'bkcharts==0.2', 'astropy==3.0.2', 'pyspider==0.3.10', 'snowballstemmer==1.2.1', 'locket==0.2.0', 'tblib==1.3.2', 'bleach==2.1.3', 'datashape==0.5.4', 'jupyter==1.0.0', 'pyflakes==1.6.0', 'pathlib2==2.3.2', 'pandas==0.23.0', 'tables==3.4.3', 'testpath==0.3.1', 'alabaster==0.7.10', 'mkl-fft==1.0.0', 'anaconda-project==0.8.2', 'imageio==2.3.0', 'h5py==2.7.1', 'PySocks==1.6.8', 'nose==1.3.7', 'mistune==0.8.3', 'python-dateutil==2.7.3', 'widgetsnbextension==3.2.1', 'anaconda-client==1.6.14', 'PyWavelets==0.5.2', 'conda==4.5.4', 'WsgiDAV==2.4.1', 'xlrd==1.1.0', 'Sphinx==1.7.4', 'sphinxcontrib-websupport==1.0.1', 'notebook==5.5.0', 'partd==0.3.8', 'conda-build==3.10.5', 'QtAwesome==0.4.4']
 * Check or update the database collection
 * Update and sync config.py

    Welcome to use the osroom open source web system.
    osroom v1.0 Beta
    osroom website:  http://osroom.com 
    Project code download:  https://github.com/osroom/osroom 
    License: BSD2
    The operating system: Linux
    Server started...
    
 * Clean configuration cache successfully
 * Signal:(SIGCHLD, SIG_IGN).Prevent child processes from becoming [Defunct processes].(Do not need to comment out)
 * [User] add
Input username:root
Input email:asialiugf@sina.com
Input password(Password at least 8 characters):
 * Create root role...
Create root user role successfully
 * Create root user...
 * Create a root user role successfully
 * Create the average user role...
 * Create a generic user role successfully
The basic information is as follows
Username: root
Email: asialiugf@sina.com
User role: Root
Password: fo****book
End
[root@iZ23psatkqsZ osroom]# 
```
### osroom开启
```python
[root@iZ23psatkqsZ osroom]# python start.py runserver --host 0.0.0.0 --port 5000           
 * Check or update Python third-party libraries
 * Update pip...
Looking in indexes: http://mirrors.aliyun.com/pypi/simple/
Requirement already up-to-date: pip in /root/anaconda3/lib/python3.6/site-packages (18.0)
 * Now don't need python library:
['dask==0.17.5', 'ipywidgets==7.2.1', 'python-dateutil==2.7.3', 'nbformat==4.4.0', 'backports.shutil-get-terminal-size==1.0.0', 'terminado==0.8.1', 'defusedxml==0.5.0', 'distributed==1.21.8', 'docutils==0.14', 'msgpack==0.5.6', 'pyzmq==17.0.0', 'Sphinx==1.7.4', 'jupyter-console==5.2.0', 'locket==0.2.0', 'pep8==1.7.1', 'sympy==1.1.1', 'bkcharts==0.2', 'html5lib==1.0.1', 'XlsxWriter==1.0.4', 'h5py==2.7.1', 'clyent==1.2.2', 'navigator-updater==0.2.1', 'heapdict==1.0.0', 'jsmin==2.2.2', 'pytest-astropy==0.3.0', 'qtconsole==4.3.1', 'pyparsing==2.2.0', 'isort==4.3.4', 'partd==0.3.8', 'scipy==1.1.0', 'tables==3.4.3', 'networkx==2.1', 'snowballstemmer==1.2.1', 'Pygments==2.2.0', 'pickleshare==0.7.4', 'mccabe==0.6.1', 'unicodecsv==0.14.1', 'tblib==1.3.2', 'openpyxl==2.5.3', 'toolz==0.9.0', 'statsmodels==0.9.0', 'mpmath==1.0.0', 'gevent==1.3.0', 'pkginfo==1.4.2', 'parso==0.2.0', 'alabaster==0.7.10', 'filelock==3.0.4', 'py==1.5.3', 'datashape==0.5.4', 'WsgiDAV==2.4.1', 'pandas==0.23.0', 'blaze==0.11.3', 'decorator==4.3.0', 'PySocks==1.6.8', 'testpath==0.3.1', 'Send2Trash==1.5.0', 'wcwidth==0.1.7', 'mistune==0.8.3', 'jupyterlab-launcher==0.10.5', 'pyflakes==1.6.0', 'simplegeneric==0.8.1', 'notebook==5.5.0', 'pytest-doctestplus==0.1.3', 'singledispatch==3.4.0.3', 'scikit-image==0.13.1', 'bokeh==0.12.16', 'jdcal==1.4', 'pytest==3.5.1', 'gmpy2==2.0.8', 'ply==3.11', 'prompt-toolkit==1.0.15', 'cssselect==1.0.3', 'fastcache==1.0.2', 'pluggy==0.6.0', 'pyspider==0.3.10', 'pytest-openfiles==0.3.0', 'webencodings==0.5.1', 'ipykernel==4.8.2', 'pycodestyle==2.4.0', 'jupyter==1.0.0', 'matplotlib==2.2.2', 'pyOpenSSL==18.0.0', 'more-itertools==4.1.0', 'nltk==3.3', 'pytest-remotedata==0.2.1', 'tornado==4.5.3', 'path.py==11.0.1', 'astropy==3.0.2', 'ptyprocess==0.5.2', 'numba==0.38.0', 'xlwt==1.3.0', 'numpy==1.14.3', 'et-xmlfile==1.0.1', 'widgetsnbextension==3.2.1', 'packaging==17.1', 'rope==0.10.7', 'bitarray==0.8.1', 'typing==3.6.4', 'beautifulsoup4==4.6.0', 'scikit-learn==0.19.1', 'patsy==0.5.0', 'cytoolz==0.9.0.1', 'imagesize==1.0.0', 'numpydoc==0.8.0', 'mkl-random==1.0.1', 'odo==0.5.1', 'PyWavelets==0.5.2', 'sphinxcontrib-websupport==1.0.1', 'jsonschema==2.6.0', 'conda-verify==2.0.0', 'cycler==0.10.0', 'entrypoints==0.2.3', 'Bottleneck==1.2.1', 'kiwisolver==1.0.1', 'lazy-object-proxy==1.3.1', 'msgpack-python==0.5.6', 'conda==4.5.4', 'pyquery==1.4.0', 'contextlib2==0.5.5', 'multipledispatch==0.5.0', 'pandocfilters==1.4.2', 'QtPy==1.4.1', 'jupyter-core==4.4.0', 'pyodbc==4.0.23', 'greenlet==0.4.13', 'xlrd==1.1.0', 'nose==1.3.7', 'Flask-Cors==3.0.4', 'pylint==1.8.4', 'anaconda-project==0.8.2', 'ruamel-yaml==0.15.35', 'sortedcollections==0.6.1', 'ipython-genutils==0.2.0', 'pycosat==0.6.3', 'boto==2.48.0', 'bleach==2.1.3', 'SQLAlchemy==1.2.7', 'u-msgpack-python==2.5.0', 'jupyterlab==0.32.1', 'colorama==0.3.9', 'mkl-fft==1.0.0', 'sortedcontainers==1.5.10', 'astroid==1.6.3', 'llvmlite==0.23.1', 'anaconda-navigator==1.8.7', 'numexpr==2.6.5', 'cloudpickle==0.5.3', 'pathlib2==2.3.2', 'pycurl==7.43.0.1', 'attrs==18.1.0', 'ipython==6.4.0', 'jupyter-client==5.2.3', 'wrapt==1.10.11', 'pexpect==4.5.0', 'backcall==0.1.0', 'QtAwesome==0.4.4', 'zict==0.1.3', 'seaborn==0.8.1', 'conda-build==3.10.5', 'jedi==0.12.0', 'glob2==0.6', 'pytest-arraydiff==0.2', 'Cython==0.28.2', 'nbconvert==5.3.1', 'spyder==3.2.8', 'imageio==2.3.0', 'traitlets==4.3.2', 'anaconda-client==1.6.14']
 * Check or update the database collection
 * Update and sync config.py

    Welcome to use the osroom open source web system.
    osroom v1.0 Beta
    osroom website:  http://osroom.com 
    Project code download:  https://github.com/osroom/osroom 
    License: BSD2
    The operating system: Linux
    Server started...
    
 * Clean configuration cache successfully
 * Signal:(SIGCHLD, SIG_IGN).Prevent child processes from becoming [Defunct processes].(Do not need to comment out)
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
106.37.98.0 - - [10/Aug/2018 23:33:29] "GET / HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:29] "GET /theme/static/css/bootstrap.min.css?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:29] "GET /theme/static/css/font-awesome.min.css?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:29] "GET /theme/static/css/bootstrapValidator.min.css?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:29] "GET /theme/static/css/style.css?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:29] "GET /theme/static/css/osroom.css?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:29] "GET /theme/static/css/osroom-m.css?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:29] "GET /theme/static/css/checkbox.css?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:29] "GET /theme/static/css/highlight-style-github.css?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:29] "GET /theme/static/js/vue.min.js?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:30] "GET /theme/static/js/jquery.min.js?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:30] "GET /theme/static/js/osroom.js?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:31] "GET /theme/static/js/markdown/marked.min.js?v=20180404041316 HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:31] "GET /static/sys_imgs/osroom-logo.png HTTP/1.1" 200 -
106.37.98.0 - - [10/Aug/2018 23:33:31] "GET /theme/static/js/highlight.js?v=20180404041316 HTTP/1.1" 200 -
```
