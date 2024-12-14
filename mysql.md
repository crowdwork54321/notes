### mysql 常用知识点

1. Mysql yum源地址

   https://dev.mysql.com/downloads/repo/yum/

2. yum 源安装mysql过程

   https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/

3. mysql官方文档

   https://downloads.mysql.com/docs/refman-5.7-en.pdf

4. 更改mysql 用户密码

   ~~~
   1. set password for 'root'@‘localhost' = 'password';
   2. alter user 'root'@'localhost' identified by 'newpassword';
   3. use mysql; update user set authentication_string=password('newpassword') where user='root' and host='localhost'; flush privileges;
   ~~~

5. mysql 设置字符集在my.cnf中添加

   ~~~
   [client]
   default-character-set = utf8mb4
   
   [mysql]
   default-character-set = utf8mb4
   
   [mysqld]
   character-set-client-handshake = FALSE
   character-set-server = utf8mb4
   collation-server = utf8mb4_unicode_ci
   init_connect='SET NAMES utf8mb4'
   ~~~
   
6. windows 安装解压版mysql5.7可以参考如下配置：9*/#7
   https://www.cnblogs.com/kunjian/p/11202636.html
   https://blog.csdn.net/u012145252/article/details/80655367

7. mysql5.7 window版本与之前的版本存在如下不一致：
   1.0 mysql57 没有data目录， 没有mysql数据库， 没有默认的my.ini配置文件，路径分割符使用\\。

8. windows 安装Mysql5.7 社区版
    1.0 mysql 社区版官方下载地址：https://dev.mysql.com/downloads/，下载解压版本；
    2.0 配置环境变量，在path中将mysql解压包的bin文件夹路径添加到pah路径中；
    3.0 在mysql解压文件夹中创建my.ini配置文件，内容如下：
        [mysqld]
        # 设置mysql的工作目录，安装包解压后的路径
        basedir=C:\\workstation\\mysql57
        
        # 数据存放目录data，需要自行新建
        # 也可以使用mysqld --initialize-insecure 命令后也会自动在根目录中生成data目录
        datadir=C:\\workstation\\mysql57\\data
        
        # 默认连接端口3306，正式环境一般都会修改
        port=3306
        
        # 设置mysql默认字符集为utf-8
        character-set-server=utf8
        
        character-set-server = utf8mb4
        collation-server = utf8mb4_unicode_ci
        
        [client]
        # 客户端配置
        default-character-set = utf8mb4
    
    4.0 执行mysqld --initialize --user=mysql --console 命令，初始化mysql，并将自动生成root的账号密码显示在控制台上。
    5.0 执行mysqld --install mysql 创建服务名为Mysql的服务；mysqld --remove mysql 删除服务名为Mysql的服务；
    6.0 使用net start/stop mysql 启动、停止mysql服务；
    7.0 使用 alter user 'root'@'localhost' identified by 'abc123.a';更改root账户密码；

  
9. mysql 给指定的用户授权：
  `GRANT ALL ON menagerie.* TO 'your_mysql_name'@'your_client_host';`

10. 




