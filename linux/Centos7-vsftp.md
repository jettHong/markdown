# Centos 7 使用yum方式安装配置 vsftpd 服务

0. 配置163源

     

1. yum安装

   ```sh
   yum -y install vsftpd
   ```

2. 创建相应用户

   添加用户，指定分组，指定home目录，设置不能登陆

   ```sh
   useradd -g root -d /home/ftp -s /sbin/nologin cyfc
   
   # 设置密码
   passwd cyfc
   
   # 设置权限
   chown -R cyfc:root /home/ftp/
   chmod -R 777 /home/ftp/
   ```

3. 配置vsftpd文件

   ```shell
   vi /etc/vsftpd/vsftpd.conf
   ```

   添加修改以下配置信息

   ```ini
   # 设置不能匿名登陆
   anonymous_enable=NO
   
   # 不启用ascii模式
   ascii_upload_enable=NO
   ascii_download_enable=NO
   
   # 是否允许递归查询，消耗性能建议关闭
   ls_recurse_enable=NO
   
   # 如果设置为YES，则vsftpd将以独立模式运行，由vsftpd自己监听和处理IPv4端口的连接请求
   # 与 listen_ipv6 相冲突！！！
   listen=YES
   
   # 与 listen 相冲突！！！（如要同时监听IPv4和IPv6，则必须运行两套vsftpd，采用两套配置文件，同时确保其中有一个监听选项是被注释掉的）
   #listen_ipv6=YES
   
   # 指定监听端口
   listen_port=30020
   
   # 被动模式
   pasv_enable=YES
   pasv_min_port=30030
   pasv_max_port=30080
   # 外网地址
   pasv_address=192.168.81.152
   pasv_addr_resolve=YES
   # 检查数据连接和控制连接是来自同一个IP地址
   pasv_promiscuous=YES
   
   # 指定根目录
   local_root=/home/ftp
   # 无法离开根目录
   chroot_local_user=YES
   # 指定可以上传文件
   allow_writeable_chroot=YES
   ```

   

4. 配置系统权限

   SELinux

   ```sh
   # 查看权限
   getsebool -a | grep ftp
   
   # 开放指定权限
   sudo setsebool -P tftp_home_dir          1
   sudo setsebool -P allow_ftpd_full_access 1
   
   # 无脑关闭
   setenforce 0
   ```

   防火墙

   ```sh
   firewall-cmd --zone=public --add-port=30020/tcp --permanent
   firewall-cmd --zone=public --add-port=30030-30080/tcp --permanent
   systemctl restart firewalld
   ```

   pam.d

   ```sh
   vi /etc/pam.d/vsftpd
   ```

   屏蔽`pam_shells.so`，添加`pam_nologin.so`

   ```ini
   #auth       required    pam_shells.so
   auth       required     pam_nologin.so
   ```

   

5. TODO 虚拟用户

6. EOF