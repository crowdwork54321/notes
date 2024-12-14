#!/bin/bash

yum -y install gcc gcc-c++ libxml2 libxml2-devel bzip2 \
bzip2-devel libmcrypt libmcrypt-devel openssl openssl-devel \
libcurl-devel libjpeg-devel libpng-devel freetype-devel readline \
readline-devel libxslt-devel perl perl-devel psmisc.x86_64 \
recode recode-devel libtidy libtidy-devel  epel-release libmcrypt-devel autoconf

sleep 1

cd /usr/local/src/
curl -O https://www.php.net/distributions/php-7.2.22.tar.gz
sleep 1
tar -zxvf php-7.2.22.tar.gz
cd php-7.2.22
useradd -s /sbin/nologin -M www
yum -y install libmcrypt-devel epel-release libmcrypt wget git
sleep 1

./configure --prefix=/etc/php --sysconfdir=/etc/php/etc \
--with-config-file-path=/etc/php/etc/  --with-fpm-user=www \
--with-fpm-group=www --enable-fpm --with-mysql=mysqlnd \
--with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-mhash  \
--with-openssl --with-zlib --with-bz2 --with-curl --with-libxml-dir \
--with-gd --with-jpeg-dir  --with-png-dir --with-zlib --enable-mbstring \
--with-mcrypt --enable-sockets --with-iconv-dir   --enable-zip \
--with-pcre-dir --with-pear --enable-session  --enable-gd-native-ttf  \
--enable-xml --with-freetype-dir --enable-gd-jis-conv --enable-inline-optimization \
--enable-shared  --enable-soap --enable-bcmath --enable-sysvmsg --enable-sysvsem \
--enable-sysvshm  --enable-mbregex --enable-pcntl --with-xmlrpc --with-gettext \
--enable-exif --with-readline   --enable-ftp   --enable-redis
sleep 1

make && make install

sleep 1
cp php.ini-production /etc/php/etc/php.ini
cp /usr/local/src/php-7.2.22/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
mv /etc/php/etc/php-fpm.conf.default /etc/php/etc/php-fpm.conf
cp /etc/php/etc/php-fpm.d/www.conf.default /etc/php/etc/php-fpm.d/www.conf
chmod 755 /etc/init.d/php-fpm
ln -s /etc/php/bin/php /usr/sbin/php
ln -s /etc/php/sbin/php-fpm /usr/sbin/php-fpm

chkconfig --add php-fpm
chkconfig php-fpm on
service php-fpm start

echo "export PATH=/etc/php/bin:$PATH"  >> /etc/profile
source /etc/profile

# php-redis扩展
cd /usr/local/src
    wget  https://pecl.php.net/get/redis-5.2.0.tgz
    tar zvxf  redis-5.2.0.tgz
    cd redis-5.2.0
    /etc/php/bin/phpize
    ./configure --with-php-config=/etc/php/bin/php-config
    make 
    make install
# php-swool扩展
cd /usr/local/src
    wget https://pecl.php.net/get/swoole-4.4.14.tgz
    tar -xf  swoole-4.4.14.tgz
    cd swoole-4.4.14
        /etc/php/bin/phpize
        ./configure --enable-openssl --with-openssl-dir=/usr/include/openssl/  --enable-http2  --with-php-config=/etc/php/bin/php-config
        make
        make install
# php-kafka扩展
cd /usr/local/src
    git clone https://github.com/edenhill/librdkafka.git
    cd librdkafka/
    ./configure
    make && make install
    wget  https://pecl.php.net/get/rdkafka-4.0.4.tgz
    tar zvxf rdkafka-4.0.4.tgz
    cd rdkafka-4.0.4
    /etc/php/bin/phpize
    ./configure --with-php-config=/etc/php/bin/php-config
    make && make install

echo "
extension =/etc/php/lib/php/extensions/no-debug-non-zts-20170718/redis.so
extension =/etc/php/lib/php/extensions/no-debug-non-zts-20170718/swoole.so
#extension =/etc/php/lib/php/extensions/no-debug-non-zts-20170718/amqp.so
extension =/etc/php/lib/php/extensions/no-debug-non-zts-20170718/rdkafka.so
swoole.enable_coroutine = On
" >>/etc/php/etc/php.ini
