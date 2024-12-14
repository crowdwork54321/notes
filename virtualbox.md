virtual box 共享文件夹的设置 
1.0  yum -y install g++ gcc gcc-c++ make kernel-*  bzip2*  #安装依赖;
2.0  在virtaulbox 管理页面>设置>存储>存储介质>挂载VBoxGuestAdditions.iso 镜像（镜像在virtualbox的安装路径下）
3.0  mount /dev/cdrom /mnt  #挂载；
4.0  ./VBoxLinuxAdditions.run   #执行 ；
5.0  在virtualbox 配置共享目录；
