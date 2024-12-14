1. swoft 开启热重载：
php swoftcli.phar run -c http:start -b bin/swoft 

2. swoft 常用命令：

   Php swoftcli.phar gen:ctrl   生成一个http  controller;

3. swoft的配置文件在config目录下， 但是要在app/bean中截入config配置； 

4. swoft生成实体命令(swoft/devtool下的命令，需要安装swoft/devtool插件）

   ~~~
   php ./bin/swoft entity:create -y   数据库中所有的表都生成实体文件； 
   php ./bin/swoft entity:create -y table tableName  生成tableName表对应的实体； 
   ~~~

5. Swoft 数据迁移常用命令：

   ~~~
    php bin/swoft migrate:create migration_file_name;
    php bin/swoft migrate:up migration_file_name 执行迁移文件； 不指定migration_file_name指执行所有未执行的迁移文件； 
    php bin/swoft migrate:down migration_file_name 回滚某个迁移文件，不指定回滚上一个迁移文件； 
    php bin/swoft migrate:history
   ~~~
    
 
  
6. Docker 安装swoft项目的步骤：
   ~~~
   宿主机安装git,并执行  git clone https://github.com/swoft-cloud/swoft
   进入到项目内部，复制得到.env文件   cd swoft  && cp .env.example  .env
   利用swoft/swoft image 创建名为swoft的容器   docker run -itd --name swoft -p 8888:18306 -v $(pwd):/var/www/swoft --entrypoint="" swoft/swoft  /bin/bash
   进入容器   docker exec -it swoft /bin/bash 
   安装项目依赖 Composer install 

   参考：https://segmentfault.com/a/1190000018235993
   ~~~
7. Docker 搭建swoft开发环增
~~~
   0. docker network create -d bridge swoft 
   1. docker run -itd --name redis -p 6379:6379 --network swoft  redis
   2. docker run -itd --name redis -p 6379:6379 --network swoft  redis
   3. docker run -itd  --name mysql -p 3306:3306 --network swoft  --privileged=true  -e MYSQL_ROOT_PASSWORD=123456  -v /Users/more/Documents/docker/mysql/etc/conf.d:/etc/mysql/conf.d -v /Users/more/Documents/docker/mysql/data:/var/lib/mysql mysql:5.7

   4. docker run -itd --name swoft -p 8888:18306 -v $(pwd):/var/www/swoft --entrypoint="" --network swoft swoft/swoft  /bin/bash
~~~
   



0. 创建桥接网络 swoft
~~~
docker network create -d bridge swoft
~~~

1.安装zookeeper(使用zookeeper<推荐>和wurstmeister/zookeeper)
~~~
docker run -d --restart=always --log-driver json-file --log-opt max-size=100m --log-opt max-file=2  --name zookeeper --network swoft -p 2181:2181  wurstmeister/zookeeper
~~~
~~~
docker run -d --restart=always --log-driver json-file --log-opt max-size=100m --log-opt max-file=2  --name zookeeper --network swoft -p 2181:2181  zookeeper
~~~

2.安装kafka
~~~
docker run -d --restart=always --log-driver json-file --log-opt max-size=100m --log-opt max-file=2 --name kafka --network swoft -p 9092:9092 -e KAFKA_BROKER_ID=0 -e KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181/kafka -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://kafka:9092 -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -v /etc/localtime:/etc/localtime wurstmeister/kafka
~~~

3.创建redis容器
~~~
docker run -itd --name redis -p 6379:6379 --network swoft  redis  
~~~
~~~
docker run -itd -v ~/workstation/data/redis:/usr/local/etc/redis --network swoft --name redis redis redis-server /usr/local/etc/redis/redis.conf(使用自定义的redis.conf)
~~~

4 创建mysql容器(windows路径为\,linux路径为/)
~~~
docker run -itd --name mysql -p 3306:3306 --network swoft  --privileged=true  -e MYSQL_ROOT_PASSWORD=123456  -v E:\docker\mysql\conf.d:/etc/mysql/conf.d -v E:\docker\mysql\data\mysql:/var/lib/mysql mysql:5.7
~~~

5.创建项止(windows子系统中文件路径从子系统的家目录开如， 所有项目均放在子系统的家目录)
~~~
docker run -itd --name admin -p 81:18306 -v ~/phpproject/admin:/var/www/swoft --entrypoint="" --network swoft swoft/swoft  /bin/bash
~~~

docker安装redis集群（使用grokzen/redis-cluster镜像）（三主三从  7000到7002为主）
~~~
 docker run -d  -e "IP=0.0.0.0" -p 7000-7005:7000-7005  --name redis-cluster --network swoft grokzen/redis-cluster:7.0.1 
~~~



swoft 容器安装rdkafka
~~~

1. git clone https://github.com/edenhill/librdkafka.git  && cd librdkafka/ && ./configure && make && make install 
2. ln -s /usr/local/lib/librdkafka.so.1 /usr/lib/
3. git clone https://github.com/arnaud-lb/php-rdkafka.git
4. cd php-rdkafka
5. phpize
6. ./configure --with-php-config=/usr/local/bin/php-config
7. make && make install
~~~


docker 安装es
~~~
docker run --name es -p 9200:9200 -p 9300:9300  --network swoft --privileged=true -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx128m" -v ~/Documents/docker/es/config:/usr/share/elasticsearch/config -v ~/Documents/docker/es/data:/usr/share/elasticsearch/data -v ~/Documents/docker/es/plugins:/usr/share/elasticsearch/plugins -d elasticsearch:7.10.1
~~~

docker 安装kafka Manager
~~~
docker run -d --name km --network swoft -p 9000:9000 -eZK_HOSTS=zookeeper  kafkamanager/kafka-manager
~~~

docker php 安装pcntl
~~~
docker-php-ext-install pcntl
~~~




