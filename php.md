### Centos 安装php7.2

1. ----使用webtatic源----
   ``` yum -y install epel-release
   rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
   yum -y install php72w-fpm php72w-cli php72w-opcache php72w-common php72w-pdo php72w-mbstring php72w-xml php72w-gd php72w-mysqlnd php72w-intl php72w-soap php72w-devel
   systemctl start php-fpm
   systemctl enable php-fpm
   ```

2. ----使用fedora源----

   ``` yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
   yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
   yum install yum-utils
   subscription-manager repos --enable=rhel-7-server-optional-rpms
   yum-config-manager --enable remi-php72
   yum update
   yum search php72 | more
   yum install php72 php72-php-fpm php72-php-gd php72-php-json php72-php-mbstring php72-php-mysqlnd php72-php-xml php72-php-xmlrpc php72-php-opcache
   ```



3. Mariadb 

   ``` create or replace user username@'localhost' identified by 'password';
   删除用户
   drop user if exists username@'localhost' 
   给用户授权
   grant select,insert,update on wangeqiuapp.* to 'username'@'%';
   修改用户密码
   SET PASSWORD [FOR user] = PASSWORD('some password')
   SET PASSWORD FOR 'bob'@'%.loc.gov' = PASSWORD('newpass');修改'bob'@'%.loc.gov'账号密码
   SET PASSWORD = PASSWORD('newpass'); 修改root账号密码
   参考地址：https://mariadb.com/kb/en/library/create-user/
   ```

4. Mariadb 命令行开启慢日志
   ``` set global slow_query_log = ON;
   SET GLOBAL long_query_time = 2;
   SET GLOBAL log_queries_not_using_indexes = ON;-- 是否打开看个人需要
   set global log_output='TABLE';
   ```

5. 配置文件开启慢日导=志
   ``` slow_query_log=ON|OFF      #开启或关闭慢日志查询
   log_output = FILE          #以文件形式输出
   long_query_time = 2        #超过2秒中即为慢日志
   slow_query_log_file=/usr/local/mysql/mysqld_slow.log
   log_error=/var/log/mariadb/mariadb.log
   log_warnings=1
   ```

   ``` character-set-client-handshake = FALSE
   
   ```

6. 配置字符集

   ``` character-set-server = utf8mb4
   collation-server = utf8mb4_unicode_ci
   init_connect='SET  NAMES utf8mb4'
   ```

7. Mysql导出数据
   ``` 命令行下具体用法如下： mysqldump -h 主机id -u用戶名 -p 數據库名 表名 > 脚本名;
   1、导出數據库為dbname的表结构（其中用戶名為root,密码為dbpasswd,生成的脚本名為db.sql）
   mysqldump -h x.x.x.x -u root -p -d dbname > db.sql;
   2、导出數據库為dbname某张表(test)结构
   mysqldump -uroot -p -d dbname test > db.sql;
   3、导出數據库為dbname所有表结构及表數據（不加-d）
   mysqldump -h x.x.x.x -u root -p  dbname > db.sql;
   4、导出數據库為dbname某张表(test)结构及表數據（不加-d）
   mysqldump  -h x.x.x.x  -u root -p dbname test > db.sql;
   ```

8. REDIS
   https://www.cnblogs.com/zzming/p/10018096.html

9. nginx配置文件：(nginx官网）
   https://www.nginx.com/resources/wiki/

   ``` 
   server {
       listen      80 default_server;
       listen      443 ssl;
       server_name riyisolar.com www.riyisolar.com;
       #access_log  /var/log/nginx/host.access.log  main;
       root   /usr/share/nginx/html/wangeqiuapp/public;
       index  index.html index.htm index.php;
       location / {
           try_files $uri $uri/ /index.php?$query_string;
       }
       ssl_certificate /usr/share/nginx/cert/riyisolar.com.pem;
       ssl_certificate_key /usr/share/nginx/cert/riyisolar.com.key;
       ssl_session_timeout 5m;
       ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
       ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
       ssl_prefer_server_ciphers on;
       error_page   500 502 503 504  /50x.html;
       location = /50x.html {
           root   /usr/share/nginx/html;
       }
   
       # proxy the PHP scripts to Apache listening on 127.0.0.1:80
       #location ~ \.php$ {
       #    proxy_pass   http://127.0.0.1;
       #}
   
       # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
       location ~ \.php$ {
           fastcgi_pass   127.0.0.1:9000;
           fastcgi_index  index.php;
           fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
           include        fastcgi_params;
       }
   }
   
   ```

10. Composer
    ``` centos/ubuntu 安装composer
    curl -sS https://getcomposer.org/installer | php
    mv composer.phar /usr/local/bin/composer
    ```

11. composer 常用命令
    ``` composer install  
    composer update  
    composer dump-autoload 
    ```
    
12. laravel 常用命令
    ``` php artisan db:seed
    php artisan migrate  
    php artisan make:model Models\Movie   (windows环境下在，在app\models 下创建Movie 模型_);
    php artisan config:cache  缓存配置文件
    php artisan config:clear  清空配置文件
    php artisan admin:make MovieController --model=App\Models\Movie  创建后台资源控制器
    php artisan make:controller TestController --model=App\Models\Test  --resource
    php artisan key:generate   生成应用的app_key; 
    php artisan migrate:reset  回滚所有的迁移；
    php artisan migrate:fresh  删除所有的表，然后执行迁移文件；
    php artisan migrate:refresh   回滚所有的迁移文件，然后再次执行所有的迁移文件；
    php artisan migrate:fresh  删除所有的表，然后执行所有migration文件；
    php artisan list  列出所有的artisan 命令
    php artisan help  命令名称
    php artisan passport:install 会自动创建客户端和密钥， 同时也会创建公钥匙， 公钥匙保存在database中；
    ```

13. mysl5.7 慢日志
    ``` show VARIABLES like '%slow%';  查看mysql慢日起配置；
    slow_query_log=1
    slow_query_log_file=/var/log/slow-mysql-query.log
    long_query_time=1
    mysql5.7的错误日导
    log-error=/var/log/mysqld.log
    pid-file=/var/run/mysqld/mysqld.pid
    https://dev.mysql.com/doc/refman/5.7/en/charset-applications.html
    ```

14. mysql 数据库密码
    ``` mysqladmin -u root password "new_password"
    更新数据库密码：
    mysql:  
    update user set password = password("kde812a35aef"),authentication_string=password("kde812a35aef") where user='root';
    flush privileges;
    CREATE USER 'villiam_slave'@'localhost' IDENTIFIED BY 'abc123.a';
    GRANT REPLICATION SLAVE ON *.* TO 'villiam_slave'@'localhost';
    ```

15. mysql5.7 修改密码及配置字符集
    ``` mysql5.7 安装之后数据密码默认写在/var/log/mysqld.log中。 执行完grep 'temporary password' /var/log/mysqld.log'之后显示密码。 
    mysql 5.7 更改密码：ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
    更改字符集5.7.28
    [mysqld]
    character-set-server=utf8
    collation-server=utf8_general_ci
    ```

16. yum源安装mysql5.7
    ```` https://dev.mysql.com/downloads/repo/yum/
    2.安装rpm文件；
    rpm -Uvh mysql80-community-release-el7-3.noarch.rpm; 
    3.修改配置文件；
    在配置文件中将enable=1; 注意只能一个版本的enable=1; 
    4.安装mysql 
    	sudo yum install mysql-community-server
    ````

17.安装mysql

``` sudo yum install mysql-community-server
sudo systemctl start mysqld.service
sudo service mysqld status
sudo systemctl status mysqld.service

1. 更改mysql 密码：
   sudo grep 'temporary password' /var/log/mysqld.log
2. ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
3. CREATE USER 'wangeqiu'@'%' IDENTIFIED BY 'aFx32se&2' ;
4. GRANT select,insert,update ON db1.* TO 'jeffrey'@'localhost';
```

18. Mysql5.7 主从复制:
    ``` https://blog.csdn.net/zlf_php/article/details/88937531
    禁止主库更新；
    FLUSH TABLES WITH READ LOCK;
    解除锁定
    UNLOCK TABLES;
    #重新同步binLog
    CHANGE MASTER TO MASTER_HOST='172.17.0.0', MASTER_USER='slave', MASTER_PASSWORD='xxxxxx', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=101029836;
    ```

19. linux 環境下查看php使用的是哪個配置文件 ;

    ``` 
    php -i|grep php.ini	
    或者
    php --ini 显示加载的ini的位置； 
    ```

20. linux 环境下查看php 安装了哪些扩展

    ```
    php -m 
    ```

21. linux 环境下查看php的版本

    ```
    php -v 
    ```
22  linux 安装pear(pecl)参考 https://pear.php.net/manual/en/installation.getting.php
    wget http://pear.php.net/go-pear.phar
    $ php go-pear.phar



23. Pecl install redis
    pecl install redis 过程enable zstd compression support enable lzf compression support/enable igbinary serializer support  这三个扩展设置为no; 
    echo "extension=redis.so"  >> php.ini
24. pecl install swoole
    1. pecl install swoole  过程中所有的选项选no; 
    2. https://wiki.swoole.com/#/environment
25. window上Php 线程安全和非线程安全； IIS使用非线程安全的php， Apache使用线程安全的php；  
26. php window开发环境的搭建：
    1. 下载对应的php zip包（https://www.php.net/），iis下载非线程安全的， apache下载线程安全的。
27. php7.1之后安装mcrypt扩展：
    sudo apt install php7.2-pear //默认安装
    sudo apt install php7.2-dev //
    sudo apt install libmcrypt-dev
    sudo pecl install mcrypt-1.0.3


28. https://www.cnblogs.com/jingmin/p/6421973.html

