# CentOS 7  安装 redis

0. 配置安装源，fedora的epel仓库

   ```shell
   yum install -y epel-release
   ```

1. 安装

   ```shell
   yum install -y redis
   ```

2. 配置防火墙

   ```shell
   firewall-cmd --zone=public --add-port=6379/tcp --permanent
   ```

3. 配置自启动服务

   ```shell
   systemctl enable redis
   ```

4. 配置文件位置

   ```shell
   vi /etc/redis.conf
   ```

   重要几处修改

   ```ini
   # 注释绑定本地部分，使得其它机器也可以访问
   bind 127.0.0.1
   
   # 服务端口
   port 6379
   
   # 密码
   requirepass myRedis_2022
   ```

   
