1.更新组件
yum -y install epel-release
yum update
reboot
yum install vim wget lrzsz git 


2.安装防火墙：
yum install firewalld firewall-config   #安装firewalld
systemctl enable firewalld              #开机自启；
systemctl start firewalld               #启动防火墙；
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --reload     #重新加载配置 
firewall-cmd --state;

3.安装nginx 官网
cat>/etc/yum.repos.d/nginx.repo <<EOF
[nginx]
name=nginx repo
baseurl=https://nginx.org/packages/centos/7/\$basearch/
gpgcheck=0
enabled=1
EOF
yum update 
yum install nginx 
yum enable nginx 
yum start nginx 

4.安装mysql5.7
cat>/etc/yum.repos.d/mysql-community.repo <<EOF
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/6/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
EOF
yum update
yum install nginx
yum enable mysql 
yum start mysql 
sudo grep 'temporary password' /var/log/mysqld.log #获取root密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!'; #登陆后更改密码；
CREATE USER 'username'@'%' IDENTIFIED BY 'userpassword' ;
GRANT select,insert,update,delete ON db1.* TO 'jeffrey'@'localhost';

5.安装php7.3
yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
yum install yum-utils
subscription-manager repos --enable=rhel-7-server-optional-rpms
yum-config-manager --enable remi-php73
yum update
yum search php73 | more
yum install php73 php73-php-fpm php73-php-gd php73-php-json php73-php-mbstring php73-php-mysqlnd php73-php-xml php73-php-xmlrpc php73-php-opcache php73-php-bcmath php73-php-devel php73-php-pdo php73-php-pear
	php73-php-mysql php73-php-pdo  php73-php-pdo_mysql  php73-php-mcrypt php73-php-gd  php73-php-swoole  php73-php-redis php73-php-common php73-php-cli				  
yum enable php73-php-fpm 
yum start php73-php-fpm 

6.0 安装redis
yum --enablerepo=remi install -y redis  #使用remi的yum 源
redis-cli --version　
systemctl start redis
systemctl enable redis
#配置密码 
vim /etc/redis.conf  #打开redis的配置文件取消requirepass 前注释，并配置密码如：requirepass:redis_pass
systemctl restart redis


7.0 安装snapd
sudo yum install snapd
sudo systemctl enable --now snapd.socket
sudo ln -s /var/lib/snapd/snap /snap

8.0 安装certBot 详情查看https://certbot.eff.org/lets-encrypt/centosrhel7-nginx

9.0 安装rabbitMq，详情查看：https://gist.github.com/fernandoaleman/fe34e83781f222dfd8533b36a52dddcc


