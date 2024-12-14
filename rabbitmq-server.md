#添加vhost
rabbitmqctl add_vhost /testhost

#列出vhost
rabbitmqctl list_vhosts


#删除vhost
rabbitmqctl delete_vhost /testhost

# 添加用户  rabbitmqctl add_user {username} {password}
rabbitmqctl add_user admin 123456

#修改用户密码 rabbitmqctl change_password {username} {newpassword}
rabbitmqctl change_password admin 123456

#验证用户密码
rabbitmqctl authenticate_user admin 123456

#删除用户
rabbitmqctl delete_user admin

#列出用户
rabbitmqctl list_users

# 给用户设置标签 none management monitoring administrator 多个用,分隔
#rabbitmqctl set_user_tags {username} {tag ...}
rabbitmqctl set_user_tags admin administrator


.
#rabbitmqctl set_permissions [-p host] {user} {conf} {write} {read}
#vhost 授予用户访问权限的vhost名称 默认 /
#user 可以访问指定vhost的用户名
#conf 一个用于匹配用户在那些资源上拥有可配置的正则表达式
#write 一个用于匹配用户在那些资源上拥有可写的正则表
#read 一个用于匹配用户在那些资源上拥有可读的正则表达式
#授予admin用户可访问虚拟主机testhost，并在所有的资源上具备可配置、可写及可读的权限
rabbitmqctl set_permissions -p /testhost admin ".*" ".*" ".*"

#授予admin用户可访问虚拟主机testhost1，在以queue开头的资源上具备可配置权限、并在所有的资源上可写及可读的权限
rabbitmqctl set_permissions -p /testhost1 admin "^queue.*" ".*" ".*"

#清除权限
rabbitmqctl clear_permissions -p /testhost admin

#虚拟主机的权限
rabbitmqctl list_permissions -p /testhost

#用户权限
rabbitmqctl list_user_permissions admin
