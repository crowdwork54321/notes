### Docker搭建php开发环境

1. docker安装mysql5.7 

   1. 拉取mysql5.7 官方镜像

      ~~~
      docker pul mysql:5.7
      ~~~

      

   2. 以Mysql5.7的镜像为基础，并将本地的/home/mysql/data目录挂载到mysql容器的数据目录(/var/lib/mysql)将mysql的配置挂载到/etc/mysql/下(参考https://blog.csdn.net/woniu211111/article/details/80968154)。

      1. 创建一个临时Mysql 容器，并将其配置copy到Host主机的指定位置：

         ~~~
          docker run -itd --name mysql  mysql:5.7 bash
          docker exec -it mysql bash
          docker cp -a mysql:/etc/mysql/ /Users/villiam/Documents/docker/etc/mysql/
         ~~~

      2. 创建mysql容器，并启动

         ~~~
         docker run -itd --name mysql --privileged=true  -v /Users/villiam/Documents/docker/etc/mysql/:/etc/mysql/ -v /Users/villiam/Documents/docker/mysql/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456  -p 3306  mysql:5.7 
         
         ~~~

      3. 如果需要配置mysql相关参数可以host主机的/Users/villiam/Documents/docker/etc/mysql/目录下配置。

      

2. Docker 安装php 7.2

   1.拉取php7.2 官方镜像； 

   ~~~
   docker pull php:7.2-fpm 
   ~~~

   2.创建容器/home/nginx/html/为host的代码路径；/var/www/html为php工作目录。

   ~~~
   docker run -itd --name php --link mysql:mysql -v /Users/villiam/Documents/docker/nginx/html/:/var/www/html/ php:7.2-fpm
   ~~~

3. Docker安装nginx

   拉取nginx官方镜像

   ~~~
   docker pull nginx 
   ~~~

   2.创建nginx容器

   ~~~
   docker run -itd --name nginx --link php:php -v /Users/villiam/Documents/docker/etc/nginx/:/etc/nginx/ -v /Users/villiam/Documents/docker/nginx/html/:/var/www/html/ -v /Users/villiam/Documents/docker/nginx/log/:/var/log/nginx/ -p 8080:80  nginx
   ~~~

   3.nginx的配置

   ~~~
   server {
       listen       80;
       server_name  localhost;
   
       #charset koi8-r;
       access_log  /var/log/nginx/host.access.log  main;
   
       set $root '/var/www/html/';  #$root为php的工作目录， 尽量保证php的工作目录与nginx代码的工作目录#    相同
       location / {
           root   $root;  
           index  index.html index.htm;
       }
       
       error_page   500 502 503 504  /50x.html;
       location = /50x.html {
           root   /usr/share/nginx/html;
       }
   
   
       location ~ \.php$ {
           root           $root;#php的工作目录
           fastcgi_pass   172.17.0.3:9000;  ###php容器的内网ip  此处也可以使用php容器的名称   fastcgi_pass:php:9000
           fastcgi_index  index.php;
           fastcgi_param  SCRIPT_FILENAME  $root$fastcgi_script_name;
           include        fastcgi_params;
       }
   }
   
   
   ~~~

4. php通过Pdo扩展连接Mysql(在docker容器内通过docker-php-ext-install pdo_mysql安装pdo_mysql扩展）;

   ~~~
   <?php
   $dbms='mysql';     //数据库类型
   $host='mysql'; //数据库主机名为mysql别名或者是mysql容器内网ip
   $dbName='test';    //使用的数据库
   $user='root';      //数据库连接用户名
   $pass='123456';          //对应的密码
   $dsn="$dbms:host=$host;dbname=$dbName";
   try {
       $dbh = new \PDO($dsn, $user, $pass); //初始化一个PDO对象
       echo "连接成功<br/>";
       $dbh = null;
   } catch (\PDOException $e) {
       die ("Error!: " . $e->getMessage() . "<br/>");
   }
   
   ~~~

5. 过程中可以将php,nginx,mysql的配置copy一份放在本地，然后在挂在到对应容器的配置文件处。 

   如将/etc/mysql  ,/etc/nginx    /etc/php 的配置先copy一份到host主机， 然后在挂载到容器的对应位置。·

   /Users/villiam/Documents/docker/etc/mysql/:/etc/mysql/ 

·