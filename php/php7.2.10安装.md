### 【坑坑坑坑】

* php源码要从 github.com下，不要从php.net上下。
* 要升级curl 到 高版本 7.61.1
```
curl.x86_64                              7.61.1-1.0.cf.rhel7           @CityFan 
libcurl.x86_64                           7.61.1-1.0.cf.rhel7           @CityFan 
libcurl-devel.x86_64                     7.61.1-1.0.cf.rhel7           @CityFan 
```
* 在php源码目录 下 要执行 ./buildconf --force 才能生成./configure
* 没这个不行 /usr/lib64/libiconv.so.2 -> /usr/local/libiconv/lib/libiconv.so.2.6.0
* 没有 php.ini  要从php源码目录下 cp php.ini-development /etc/php.ini

* 安装 swoole
* 编译 hiredis后 一定要安装， 指定 export LD_LIBRARY_PATH=/usr/local/lib
* 

#### centos 7.4 库
```
[root@dev /usr/lib64]# ll | grep "Sep 22"
lrwxrwxrwx.  1 root root       23 Sep 22 20:54 libanl.so -> ../../lib64/libanl.so.1
lrwxrwxrwx.  1 root root       32 Sep 22 20:54 libBrokenLocale.so -> ../../lib64/libBrokenLocale.so.1
lrwxrwxrwx.  1 root root       15 Sep 22 20:54 libbsd.a -> libbsd-compat.a
lrwxrwxrwx.  1 root root       24 Sep 22 20:54 libcidn.so -> ../../lib64/libcidn.so.1
lrwxrwxrwx.  1 root root       25 Sep 22 20:54 libcrypt.so -> ../../lib64/libcrypt.so.1
lrwxrwxrwx.  1 root root       16 Sep 22 22:50 libcurl.so -> libcurl.so.4.5.0
lrwxrwxrwx.  1 root root       16 Sep 22 22:50 libcurl.so.4 -> libcurl.so.4.5.0
lrwxrwxrwx.  1 root root       22 Sep 22 20:54 libdl.so -> ../../lib64/libdl.so.2
lrwxrwxrwx.  1 root root       18 Sep 22 20:46 libexslt.so -> libexslt.so.0.8.17
lrwxrwxrwx.  1 root root       19 Sep 22 20:46 libgcrypt.so -> libgcrypt.so.11.8.2
lrwxrwxrwx.  1 root root       34 Sep 22 20:46 libgpg-error.so -> ../../lib64/libgpg-error.so.0.10.0
lrwxrwxrwx.  1 root root       41 Sep 22 23:02 libiconv.so.2 -> /usr/local/libiconv/lib/libiconv.so.2.6.0
lrwxrwxrwx.  1 root root       18 Sep 22 21:03 libmcrypt.so -> libmcrypt.so.4.4.8
lrwxrwxrwx.  1 root root       20 Sep 22 22:50 libmetalink.so.3 -> libmetalink.so.3.1.0
lrwxrwxrwx.  1 root root       21 Sep 22 20:54 libm.so -> ../../lib64/libm.so.6
-rwxr-xr-x.  1 root root   668640 Sep 22 23:42 libnghttp2.so
lrwxrwxrwx.  1 root root       21 Sep 22 22:50 libnghttp2.so.14 -> libnghttp2.so.14.16.1
lrwxrwxrwx.  1 root root       23 Sep 22 20:54 libnsl.so -> ../../lib64/libnsl.so.1
lrwxrwxrwx.  1 root root       30 Sep 22 20:54 libnss_compat.so -> ../../lib64/libnss_compat.so.2
lrwxrwxrwx.  1 root root       26 Sep 22 20:54 libnss_db.so -> ../../lib64/libnss_db.so.2
lrwxrwxrwx.  1 root root       27 Sep 22 20:54 libnss_dns.so -> ../../lib64/libnss_dns.so.2
lrwxrwxrwx.  1 root root       29 Sep 22 20:54 libnss_files.so -> ../../lib64/libnss_files.so.2
lrwxrwxrwx.  1 root root       30 Sep 22 20:54 libnss_hesiod.so -> ../../lib64/libnss_hesiod.so.2
lrwxrwxrwx.  1 root root       31 Sep 22 20:54 libnss_nisplus.so -> ../../lib64/libnss_nisplus.so.2
lrwxrwxrwx.  1 root root       27 Sep 22 20:54 libnss_nis.so -> ../../lib64/libnss_nis.so.2
lrwxrwxrwx.  1 root root       15 Sep 22 22:50 libpsl.so.0 -> libpsl.so.0.2.4
lrwxrwxrwx.  1 root root       26 Sep 22 20:54 libresolv.so -> ../../lib64/libresolv.so.2
lrwxrwxrwx.  1 root root       22 Sep 22 20:54 librt.so -> ../../lib64/librt.so.1
lrwxrwxrwx.  1 root root       16 Sep 22 22:50 libssh2.so -> libssh2.so.1.0.1
lrwxrwxrwx.  1 root root       16 Sep 22 22:50 libssh2.so.1 -> libssh2.so.1.0.1
lrwxrwxrwx.  1 root root       29 Sep 22 20:54 libthread_db.so -> ../../lib64/libthread_db.so.1
lrwxrwxrwx.  1 root root       24 Sep 22 20:54 libutil.so -> ../../lib64/libutil.so.1
lrwxrwxrwx.  1 root root       17 Sep 22 20:46 libxslt.so -> libxslt.so.1.1.28
drwxr-xr-x.  2 root root     4096 Sep 22 22:50 pkgconfig
[root@dev /usr/lib64]# 
```



### 从这里下载源码
* https://github.com/php/php-src/releases

### 升级 curl
* https://www.quyu.net/info/452.html
* https://serverfault.com/questions/321321/upgrade-curl-to-latest-on-centos
```
This is an old question, but it is still one the first results in google search, so I'd like post the solution that got my problem solved.

1) create a new file /etc/yum.repos.d/city-fan.repo

2) Paste the following contents:

[CityFan]
name=City Fan Repo
baseurl=http://www.city-fan.org/ftp/contrib/yum-repo/rhel$releasever/$basearch/
enabled=1
gpgcheck=0

3) type:

yum clean all
yum install curl 
4) And it's done.

```

### 这里是编译参考
* https://www.fashop.cn/guide/issue.html#%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85swoole
* https://github.com/mojisrc/fashop
```
wget https://github.com/php/php-src/archive/php-7.2.10.tar.gz
tar xvf
cd php
./buildconf --force
```
```
./configure \
--prefix=/usr/local/php-7.2.10 \
--with-config-file-path=/etc \
--enable-fpm \
--with-fpm-user=nginx  \
--with-fpm-group=nginx \
--enable-inline-optimization \
--disable-debug \
--disable-rpath \
--enable-shared  \
--enable-soap \
--with-libxml-dir \
--with-xmlrpc \
--with-openssl \
--with-mcrypt \
--with-mhash \
--with-pcre-regex \
--with-sqlite3 \
--with-zlib \
--enable-bcmath \
--with-iconv \
--with-bz2 \
--enable-calendar \
--with-curl \
--with-cdb \
--enable-dom \
--enable-exif \
--enable-fileinfo \
--enable-filter \
--with-pcre-dir \
--enable-ftp \
--with-gd \
--with-openssl-dir \
--with-jpeg-dir \
--with-png-dir \
--with-zlib-dir  \
--with-freetype-dir \
--enable-gd-native-ttf \
--enable-gd-jis-conv \
--with-gettext \
--with-gmp \
--with-mhash \
--enable-json \
--enable-mbstring \
--enable-mbregex \
--enable-mbregex-backtrack \
--with-libmbfl \
--with-onig \
--enable-pdo \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-zlib-dir \
--with-pdo-sqlite \
--with-readline \
--enable-session \
--enable-shmop \
--enable-simplexml \
--enable-sockets  \
--enable-sysvmsg \
--enable-sysvsem \
--enable-sysvshm \
--enable-wddx \
--with-libxml-dir \
--with-xsl \
--enable-zip \
--enable-mysqlnd-compression-support \
--with-pear \
--enable-opcache

```
### 修改Makefile
加上 -liconv

### libiconv.so.2
```
[root@dev /lib]# cd /usr/lib64
[root@dev /usr/lib64]# ln -s /usr/local/libiconv/lib/libiconv.so.2.6.0 libiconv.so.2
```

### phar.phar
```
抛错：

Generating phar.phar
chmod: cannot access `ext/phar/phar.phar': No such file or directory
make: [ext/phar/phar.phar] Error 1 (ignored)

Build complete.
Don't forget to run 'make test'.

此处可以忽略 不过解决办法如下

#cd  ext/phar/
#cp ./phar.php  ./phar.phar

```

```
make
make install
```

## 安装 swoole
* http://ken.weiaai.com/564.html
### 安装nghttp2
```
2.安装http2(如本来已装则跳过):
因为swoole的http2模块依赖nghttp2库所以安装nghttp2即可
wget https://github.com/nghttp2/nghttp2/releases/download/v1.30.0/nghttp2-1.30.0.tar.bz2 && \
tar -jxvf nghttp2-1.30.0.tar.bz2 && \
cd nghttp2-1.30.0 && \
./configure && \
make clean && make && make install
```
### 安装 hiredis编译
```
下载hiredis编译
 1126  wget https://github.com/redis/hiredis/archive/v0.13.3.tar.gz
 1130  cd hiredis-0.13.3/
 
make -j
sudo make install
sudo ldconfig

```
### export LD_LIBRARY_PATH=/usr/local/lib
编译 执行 swoole  会用到 hiredis 动态库

```
mkdir -p ~/build && \
cd ~/build && \
rm -rf ./swoole-src && \
curl -o ./tmp/swoole.tar.gz https://github.com/swoole/swoole-src/archive/master.tar.gz -L && \

wget https://github.com/swoole/swoole-src/archive/master.tar.gz

tar zxvf ./tmp/swoole.tar.gz && \
mv swoole-src* swoole-src && \
cd swoole-src && \
/usr/local/php-7.2.10/bin/phpize && \


./configure \
--prefix=/usr/swoole \
--with-php-config=/usr/local/php-7.2.10/bin/php-config \
--enable-coroutine \
--enable-openssl  \
--enable-http2  \
--enable-async-redis \
--enable-sockets \
--enable-mysqlnd 

&& \
make clean && make && sudo make install
```

```
[root@dev ~/build/swoole-src]# make install
Installing shared extensions:     /usr/local/php-7.2.10/lib/php/extensions/no-debug-non-zts-20170718/
Installing header files:          /usr/local/php-7.2.10/include/php/
[root@dev ~/build/swoole-src]# 
```
### 用pecl安装
```

先安装 hiredis
export LD_LIBRARY_PATH=/usr/local/lib
cd /usr/local/php-7.2.10/bin
/usr/local/php-7.2.10/bin/pecl install swoole



Build process completed successfully
Installing '/usr/local/php-7.2.10/lib/php/extensions/no-debug-non-zts-20170718/swoole.so'
Installing '/usr/local/php-7.2.10/include/php/ext/swoole/config.h'
install ok: channel://pecl.php.net/swoole-4.2.1
configuration option "php_ini" is not set to php.ini location
You should add "extension=swoole.so" to php.ini
[root@dev /usr/local/php-7.2.10/bin]# 
```

###  cp php.ini-development /etc/php.ini
php源码下的 php.ini-development 复制到 /etc/php.ini

### add "extension=swoole.so" to php.ini

### 最后执行以下命令 /usr/local/php-7.2.10/bin/php -m
```
cd /usr/local/php-7.2.10/bin]# 
/usr/local/php-7.2.10/bin/php --ini
/usr/local/php-7.2.10/bin/php -m
```



