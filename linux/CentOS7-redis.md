# CentOS 7  安装 redis

### yum安装法：

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
   # 注释绑定 127.0.0.1 本地部分，使得其它机器也可以访问
   # 是绑定本机的IP地址，（准确的是：本机的网卡对应的IP地址，每一个网卡都有一个IP地址），而不是redis允许来自其他计算机的IP地址。
   # 如果指定了bind，则说明只允许来自指定网卡的Redis请求。如果没有指定，就说明可以接受来自任意一个网卡的Redis请求。
   bind 0.0.0.0
   
   # 服务端口
   port 6379
   
   # 密码
   requirepass myRedis_2022
   
   # 后台运行
   daemonize yes
   
   # 最大内存
   maxmemory 2gb
   ```

5. ~~（不需要）设置自启动~~

   编辑配置文件

   ```sh
   vi /etc/systemd/system/redis.service
   ```

   填入以下内容

   ```sh
   [Unit]
   Description=Redis
   After=network.target
   
   [Service]
   Type=forking
   ExecStart=/usr/bin/redis-server /etc/redis.conf
   ExecReload=/usr/bin/redis-server -s reload
   ExecStop=/usr/bin/redis-server -s stop
   PrivateTmp=true
   
   [Install]
   WantedBy=multi-user.target
   ```

   ```sh
   systemctl enable redis                  # 开机自启redis服务
   systemctl disable redis                 # 取消开机自启
   systemctl start redis.service　         # 启动redis服务
   systemctl stop redis.service　          # 停止服务
   systemctl restart redis.service　       # 重新启动服务
   systemctl status redis.service          # 查看服务当前状态
   systemctl list-units --type=service     # 查看所有已启动的服务
   systemctl daemon-reload                 # 加载服务配置文件
   ```

   

### 编译安装法：

0. 官方下载

    ```bash
    cd    /usr/local
    wget https://download.redis.io/redis-stable.tar.gz
    # http://download.redis.io/releases/redis-6.2.7.tar.gz
    ```

1. 安装依赖包

   ```bash
   yum -y install gcc-c++ make python3
   ```

2. 编译安装

   ```bash
   # 解压文件
   tar -xzvf redis-stable.tar.gz
   
   # 进入解压后目录
   cd redis-stable
   
   # 安装到编译成功的二进制文件到/usr/local/bin中
   mkdir /usr/local/redis
   make &&  make PREFIX=/usr/local/redis install
   
   # 复制配置文件
   cp /usr/local/redis-stable/redis.conf /etc/redis.conf
   ```

3. 配置自启动

   ```sh
   vi /etc/systemd/system/redis.service
   
   # ===== 输入以下信息
   [Unit]
   Description=Redis
   After=network.target
   
   [Service]
   Type=forking
   ExecStart=/usr/local/redis/bin/redis-server /etc/redis.conf
   ExecReload=/usr/local/redis/bin/redis-server -s reload
   ExecStop=/usr/local/redis/bin/redis-server -s stop
   PrivateTmp=true
   
   [Install]
   WantedBy=multi-user.target
   ```

   

4. 配置防火墙

   ```sh
   firewall-cmd --zone=public --add-port=6379/tcp --permanent
   
   # 重启防火墙以使配置即时生效
   systemctl restart firewalld
   ```

   

5. EOF



REF：

[Linux安装redis并用systemctl方式配置自启动_yjLmyy的博客-CSDN博客_redis 配置systemctl 重启](https://blog.csdn.net/m0_38075171/article/details/117261954)

[Install Redis from Source | Redis](https://redis.io/docs/getting-started/installation/install-redis-from-source/)

[linux 安装redis 完整步骤 - happywish - 博客园 (cnblogs.com)](https://www.cnblogs.com/happywish/p/10944253.html)

