version: "2.1"
services:
    nginx:
        image: nginx
        container_name: nginx-learn
        restart: always
        depends_on:
          - mysql
          - php
          - redis
        ports:
            - "86:80"
        volumes: 
            - ~/Documents/phpproject/learn:/usr/share/nginx/html
            - ~/Documents/docker_learn/nginx/conf:/etc/nginx/conf.d
            - ~/Documents/docker_learn/nginx/logs:/var/log/nginx
        networks:
            - learn
    php:
        image: php:7.2.34-fpm-alpine
        container_name: php-learn
        volumes:
            - ~/Documents/phpproject/learn:/www
        networks:
            - learn
    mysql:
        image: mysql:5.7
        container_name: mysql-learn
        volumes:
            - ~/Documents/docker_learn/mysql/etc/conf.d:/var/lib/mysql
            - ~/Documents/docker_learn/mysql/etc/conf.d:/etc/mysql/conf.d
        ports:
            - "3307:3306"
        environment:
            - MYSQL_ROOT_PASSWORD=123456
        networks:
            - learn
    redis:
        image: redis
        container_name: redis-learn
        volumes:
            - ~/Documents/docker_learn/redis/data:/var/lib/redis
            - ~/Documents/docker_learn/redis/config/redis.conf:/usr/local/etc/redis/redis.conf
        ports:
            - "16379:6379"
        networks:
            - learn
networks: 
    learn:
