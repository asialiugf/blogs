## 从这里下载源码
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

