# Centos 7 使用yum方式安装配置 vsftpd 服务

0. 配置163源

   

1. yum安装

   ```shell
   yum -y install vsftpd
   ```

2. 创建相应用户

    FTP 用户主目录

   ```shell
   mkdir /ftphome
   ```

   添加用户

   TODO

   

3. 配置

   ```shell
   vi /etc/vsftp/vsftpd.conf
   ```

   添加修改以下配置信息

   ```
   listen_port=46020
   pam_service_name=vsftpd
   userlist_enable=YES
   userlist_file=/etc/vsftpd/user_list
   pasv_enable=YES
   pasv_min_port=46030
   pasv_max_port=46080
   pasv_address=59.61.83.130
   pasv_addr_resolve=YES
   anon_max_rate=512000
   local_max_rate=512000
   pasv_promiscuous=YES
   local_root=/home/cyfccph
   chroot_local_user=YES
   allow_writeable_chroot=YES
   anon_root=/home/cyfccph
   ```

   

4. 2

5. 2

6. 2

7. 2

8. 2

9. 2

10. 2

11. 2

12. 2

13. 2