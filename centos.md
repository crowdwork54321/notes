## ## Centos 常用命令

### Centos7 firewalld的常用命令
      firewall-cmd --zone=public --list-ports
      firewall-cmd --zone=public --add-port=80/tcp --permanent
      firewall-cmd --zone=public --query-port=80/tcp
      firewall-cmd --zone=public --remove-port=80/tcp --permanent
      firewall-cmd --reload

### Centos7 添加swap 交换空间：
      交换空间主要用来解决内存不足的问题， 在内存不足的时候，内存里的暂时不用的数据会暂存在
      交换空间里， 等需要用到数据的时候，对应的数据会再次加载进内存。 
      1. 查看当前是否存在swapfile  
      free -m 
      2.  创建一个5G大小的文件
      dd if=/dev/zero of=/var/swapfile bs=1G count=5
      3. 设置为swap 交换文件；
      mkswap /var/swapfile
      4.  激活并使用此交换文件
      swapon /var/swapfile
      5 设置系统启动自动激活虚拟交换文件
      /var/swapfile  swap swap  defaults 0 0


### Linux 定时清空buff/Cache;
      #!/bin/bash
      echo "开始清除缓存"
      sync;sync;sync #写入硬盘，防止数据丢失
      sleep 10#延迟10秒
      echo 1 > /proc/sys/vm/drop_caches
      echo 2 > /proc/sys/vm/drop_caches
      echo 3 > /proc/sys/vm/drop_caches
    
      Linux清空cache/buffer占用；
      swapoff -a/swapon -a;


### History -c 
      该命令可以清空本次登入的所有输出命令，但不清空.bash_history文件，所以下次登陆后，旧命令还将出现，历史命令是存在于当前用户根目录下的./bash_history文件。
      echo > ~/.bash_history  #清空历史命令；
      echo > /var/log/wtmp    #清空系统成功登陆日志；
      echo > /var/log/btmp    #清空系统登陆失败日志；




### Linux cpu资源使用
      1.CPU占用最多的前10个进程： 
      ps auxw|head -1;ps auxw|sort -rn -k3|head -10 
      2.内存消耗最多的前10个进程 
      ps auxw|head -1;ps auxw|sort -rn -k4|head -10
      3.虚拟内存使用最多的前10个进程 
      ps auxw|head -1;ps auxw|sort -rn -k5|head -10


### 定时备份数据库：

      #!/bin/bash
      # Simple script to backup MySQL databases
    
      # Parent backup directory
      backup_parent_dir="/backup/mysql"
    
      # MySQL settings
      mysql_user="root"
      mysql_password="mypass"
      mysql_server="localhost"
    
      # Create backup directory and set permissions
      backup_date=`date +%Y_%m_%d_%H_%M`
      backup_dir="${backup_parent_dir}/${backup_date}"
      echo "Backup directory: ${backup_dir}"
      mkdir -p "${backup_dir}"
      chmod 755 "${backup_dir}"
    
      # Get MySQL databases
      mysql_databases=`echo 'show databases' | mysql --host=${mysql_server} --user=${mysql_user} --password=${mysql_password} -B | sed /^Database$/d`
    
      # Backup and compress each database
      for database in $mysql_databases
      do
      if [ "${database}" == "information_schema" ] || [ "${database}" == "performance_schema" ]; then
            additional_mysqldump_params="--skip-lock-tables"
      else
            additional_mysqldump_params=""
      fi
      echo "Creating backup of \"${database}\" database"
      mysqldump ${additional_mysqldump_params} --host=${mysql_server} --user=${mysql_user} --password=${mysql_password} ${database} | gzip > "${backup_dir}/${database}.gz"
      chmod 600 "${backup_dir}/${database}.gz"
      done
    
      #Delete backups older than 3 days
      find ${backup_parent_dir} -name '*' -type d -mtime +3 -exec rm -rfv "{}" \;



### Centos后代运行命令：

1. nohup 与& 结合 

   ``` 
   nohup python3 video.py >> /dev/null 2>1& &		
   ```

2. setsid

   > setsid runs a program in a new session. The command calls fork(2) if already a process group leader. Otherwise, it executes a program in the current process.
   >
   >在一个新的会话中运行命令

   ```
    setsid ping www.ibm.com	
   ```

3. () 

   >让一个命令在括号中运行，

   ``` 
   (python3 video.py)
   ```

### 利用Linux 将PKCS#1格式的公钥转成PKCS#8的公钥 

```
openssl rsa -RSAPublicKey_in -in /home/villiamAB/Desktop/pubkey -pubout
(/home/villiamAB/Desktop/pubkey为公钥路径)
```

### Linux 更改本地dns配置文件；

``` 
vim /etc/hosts
如添加(127.0.0.1 localhost)
```

### Linux 永久更改主机名

	#### 方法1
	
		1. vim /etc/sysconfig/network 在其中添加HOSTNAME = '您想要的主机名；
  		2. vim /etc/hostname  在其中直接写上你想要的主机名即可；

#### 方法2

​	hostnamectl set-hostname ‘设置你想要的名称’



### SCP 命令远程传输

 1. 传递本地文件到远程服务器

    ```
    scp local_file remote_username@remote_ip:/path/fileName
    ```

	2. 传递远程文件到本地：

    ```
    scp remote_username@remote_ip:/path/fileName local_file
    ```

	3. 传递本地文件夹到远程服务器：

    ```
    scp -r local_folder remote_username@remote_ip:remote_folder 
    ```



### ssh 远程登陆服务器:

``` ssh root@address
ssh root@address
```



### ssh-copy-id  

​	ssh-copy-id root@hostname   使用该命令将本地的当前的用户家目录下的公钥文件copy到远程服务器hostname中的root家目录下的authentication_key中。 



###   Linux 配置公钥登陆(免密登陆)

	1. 在本地生成 rsa 公私钥对；ssh-keygen -t rsa;
 	2. ssh-copy-id  username@address  (如ssh-copy-id root@192.168.171.11) 其中username为远程服务器账号，address为远程服务器ip或本地配置的映射名；
 	3. 使用 ssh username@address 即可登陆远程服务器；

### Linux 安全

1. Linux （centos）禁此ping 操作；

   攻击者一般首先通过ping命令检测此主机或者IP是否处于活动状态 ，如果能够ping通 某个主机或者IP，那么攻击者就认为此系统处于活动状态，继而进行攻击或破坏。如果没有人能ping通机器并收到响应，那么就可以大大增强服务器的安全性，linux下可以执行如下设置。

   禁止ping操作的流程如下：

    1. 修改/etc/sysctl.conf ，在文件末尾添加一行:

       net.ipv4.icmp_echo_ignore_all = 1   #0允许ping ; 1不允许ping

       2. 修改完成后执行 sysctl -p 使新配置生效

2. 安装denyhost 防止暴力破解；

3. Linux 免密登陆；




### Linux 自定义开机启动服务 

1. 在/usr/lib/systemd/system/下创建对应的service文件；

   vim /usr/lib/systemd/system/vote.service

2. 编辑vote.service文件的内容如下：

   ``` 
   [Unit]
   Description=vote
   After=rc-local.service
    
   [Service]
   Type=forking    #后台运行
    
    
   ExecStart=/springcloud/vote start        #此处使用绝对路径 启动命令
   ExecReload=/springcloud/vote restart     #此处使用绝对路径 重启命令
   ExecStop=/springcloud/vote stop          #此处使用绝对路径 关闭命令
    
   PrivateTmp=true    #是否分配独立的临时空间（可缺省）
    
   [Install]
   WantedBy=multi-user.target
   原文链接：https://blog.csdn.net/u010448530/article/details/89249153
   ```

3. 修改vote.service文件后需要执行下面命令使其生效

   ```
   systemctl daemon-reload
   ```

4. 设置vote.service开机启动

   ```
   systemctl enable vote.service
   ```

5. CenOS7以后的操作系统可以用以下命令来启停服务：

   systemctl/services vote start

   systemctl/service vote stop

   systemctl/service vote restart
   
6. 查看centos版本信息

   cat /etc/redhat-release

### Linux 修改环境变量
1.    export PATH=$PATH:$variables   只在当前窗口有效，关闭窗口失效；
2.    cd ~ 到家目录 ， vim  .bashrc ,在.bashrc的末尾添加上对应的export=$PATH:$variable 值 ， source .bashrc
3.    cd ~到家目录， vim .profile, 在.profile的最后加上expor=$PATH:$variable值，source .profile ，重启系统；


