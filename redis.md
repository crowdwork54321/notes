Redis

1. Redis-server 常用命令：

   Info 常看redis服务端，客户端基本信息； 

   Client list 获取到当前连接到服务端的客户端列表； 
   Dbsize 获取当前的数据库的总key的个数； 

   Flushall 删除所有的数据库的数据； 

   Flushdb 删除当前数据库的数据； 

   Monitor 实时打印redis接收到的命令； 

2. Redis-cli 常用命令：

   1. ping 命令检测服务端是否开通； 
   2. Info;
   3. Config get maxclients ； 获取redis的最大连接数； 

3. Redis 授权

   config set requirepass "abc123.a" 将redis授权为abc123.a

   config get requirepass 获取设置的redis登陆密码； 

   Auth "abc123.a" 如果redis设置了密码，则需要输入以下密码才可以登陆。 

   