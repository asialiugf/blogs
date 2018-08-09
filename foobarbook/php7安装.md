### php7安装
* https://www.cnblogs.com/jkko123/p/6357670.html
```php
yum -y install gd-devel zlib-devel libjpeg-devel libpng-devel libiconv-devel freetype-devel libxml2 libxml2-devel openssl openssl-devel curl-devel libxslt-devel libmcrypt-devel mhash mcrypt
wget http://cn2.php.net/get/php-7.2.8.tar.bz2/from/this/mirror   
```
```c
./configure --prefix=/data/php7 \
--with-openssl \
--with-pcre-regex \
--with-kerberos \
--with-libdir=lib \
--with-libxml-dir \
--with-mysqli=shared,mysqlnd \
--with-pdo-mysql=shared,mysqlnd \
--with-pdo-sqlite \
--with-gd \
--with-iconv \
--with-zlib \
--with-xmlrpc \
--with-xsl \
--with-pear \
--with-gettext \
--with-curl \
--with-png-dir \
--with-jpeg-dir \
--with-freetype-dir \
--with-fpm-user=nginx \
--with-fpm-group=nginx \
--enable-mysqlnd \
--enable-zip \
--enable-inline-optimization \
--enable-shared \
--enable-libxml \
--enable-xml \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-mbregex \
--enable-mbstring \
--enable-ftp \
--enable-gd-native-ttf \
--enable-pcntl \
--enable-sockets \
--enable-soap \
--enable-session \
--enable-opcache \
--enable-fpm \
--enable-maintainer-zts \
--enable-fileinfo \
```
