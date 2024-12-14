### Ubuntu环境搭建 

1. flameshot 

   > https://www.cnblogs.com/famary/p/12133500.html
   >  https://blog.csdn.net/daerzei/article/details/84876170
   >  配置：ppa源：add-apt-repository ppa:rikmills/bionic
   >  sudo apt-get update
   >  apt install flameshot 
   >  pkill flameshot
   >  快捷方式的设置：进入系统设置中的键盘设置，选择添加自定义快捷键，设置快捷键的命令/usr/bin/flameshot gui和名称FlameShot。然后绑定一下键盘Ctrl+Alt+A;

   > 安装完成之后安装：apt install lrzsz    ; apt install zssh;
   



2. 安装中文输入法：

  > https://pinyin.sogou.com/linux/?r=pinyin

3.在postman官网或者unbutu sofeware中心安装postman

4.在unbuntu software 中心安裝telegram; 

5.安裝deepin terminal

> sudo add-apt-repository ppa:noobslab/deepin-sc
> sudo apt-get update
> sudo apt-get install deepin-terminal
> 安裝完deeping-terminal 之後還要安裝zssh
> apt install zssh

6.ubuntu 配置 php7環境； ubuntu18.04自帶php7.2的安裝源

> 執行以下命令安裝php7.2
> apt install php7.2-fpm php7.2-cli php7.2-opcache php7.2-common  php7.2-mbstring php7.2-xml php7.2-gd php7.2-mysql php7.2-intl php7.2-soap php7.2-dev  php7.2-curl 
> php -i|grep php.ini 查看php載入的配置文件 
> ubuntu18.04默認情況下不需要在cli/php.ini)打開多餘的動態模塊

   ubuntu可以利用ondrej/php源安装php
   >> 配置ondrej/php源
      sudo add-apt-repository ppa:ondrej/php
      sudo apt-get update
   
   >>安装php
      sudo apt-get install php7.3
  
   >>安装php扩展
       sudo apt-get install php7.3-<entension-name>
        如：sudo apt install php7.3-cli php7.3-fpm php7.3-json php7.3-pdo php7.3-mysql php7.3-zip php7.3-gd  php7.3-mbstring php7.3-curl php7.3-xml 
        php7.3-bcmath php7.3-json


7.安裝mariadb之後設置字符集，將字符集更爲utf8mb4：

> 在My.cnf中添加：
> [client]
> default-character-set=utf8mb4
> [mysqld]
> character-set-server=utf8mb4
> [mysql]
> default-character-set=utf8mb4

8.安装五笔拼音输入法

> sudo apt install fcitx-table-wbpy
> sudo apt update
> 重启系统
> 输入法配置中设置输入法为fcitx;
> 输入法语言支持-->键盘输入系统设置为fcitx; 

9.Ubuntu18.04 上通过自带的apt源安装的php，默认的用户为www-data; php 默认使用的是Unix-socket形式。 

> ​	fastcgi_pass unix:/var/run/php-fpm.sock;

10. Ubuntu install docker 
>   1. sudo apt update
>   2. sudo apt install apt-transport-https ca-certificates curl software-properties-common
>   3. curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
>   4. sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
>   5. sudo apt update
>   6. apt-cache policy docker-ce
>   7. sudo apt install docker-ce
>   8. sudo systemctl status docker
>    https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04

10.ubuntu 安装node; 
https://github.com/nodesource/distributions/blob/master/README.md
