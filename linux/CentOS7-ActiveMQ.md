# CentOS7 ActiveMQ

0. 下载

   官方地址：http://archive.apache.org/dist/activemq

   ```shell
   # 这里使用支持JDK8版本的最新版本
   wget http://archive.apache.org/dist/activemq/5.16.4/apache-activemq-5.16.4-bin.tar.gz
   # ActiveMQ 5.17 版本起需要 JDK11
   ```

1. 解压

   ```shell
   # 创建解压存放目录
   mkdir /usr/local/activemq
   
   tar -zxvf apache-activemq-5.16.4-bin.tar.gz -C /usr/local/activemq
   ```

   

2. 运行

   ```shell
   # 进入 activemq/bin 目录
   cd /usr/local/activemq/bin
   
   # 运行程序
   ./activemq start
   
   # 检查进程
   ps -ef|grep activemq
   ```

   

3. 配置防火墙

   

4. 设置自启动 TODO

   

5. END

REF：

[CentOS 7 安装ActiveMQ - Rozdy - 博客园 (cnblogs.com)](https://www.cnblogs.com/Rozdy/p/11497324.html)

[centos7 ActiveMQ设置开机启动-操作篇_云集千祥的博客-CSDN博客_activemq开机启动](https://blog.csdn.net/u012249177/article/details/81322874)
