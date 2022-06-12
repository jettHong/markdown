# CentOS7 ActiveMQ

0. 下载

    官方地址：http://archive.apache.org/dist/activemq

    ```shell
    cd /usr/local/
    # 这里使用支持JDK8版本的最新版本 （ActiveMQ 5.17 版本起需要 JDK11）
    wget http://archive.apache.org/dist/activemq/5.16.4/apache-activemq-5.16.4-bin.tar.gz
    ```

1. 解压

   ```shell
   # 解压
   tar -zxvf apache-activemq-5.16.4-bin.tar.gz
   # 改名
   mv apache-activemq-5.16.4 activemq
   ```

   

2. 运行

   ```shell
   # 进入 activemq/bin 目录
   cd /usr/local/activemq/bin
   
   # 运行程序
   ./activemq start
   
   # 检查进程
   ps -ef|grep activemq
   
   # 关闭程序
   ./activemq stop
   ```

   

3. 调整内存参数

   内存配置文件

   ```shell
   vi /usr/local/activemq/bin/env
   ```

   内存配置内容如下

   ```shell
   # Set jvm memory configuration (minimal/maximum amount of memory)
   ACTIVEMQ_OPTS_MEMORY="-Xms64M -Xmx1G"
   ```

   JAVA_HOME配置如下，如果不设置JAVA_HOME下面自启动会有错误。

   ```shell
   # Location of the java installation
   # Specify the location of your java installation using JAVA_HOME, or specify the
   # path to the "java" binary using JAVACMD
   # (set JAVACMD to "auto" for automatic detection)
   JAVA_HOME="/usr/local/jdk1.8.0_321"
   JAVACMD="auto"
   ```

   查看Java安装目录：

   ```shell
   whereis java
   ```

   查看编辑java的系统默认路径

   ```shell
   vi /etc/profile
   
   export JAVA_HOME=/usr/local/jdk1.8.0_321
   export JRE_HOME=${JAVA_HOME}/jre
   export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
   export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
   export PATH=$PATH:${JAVA_PATH}
   ```

   

4. 调整服务端口

   配置文件

   ```shell
   vi /usr/local/activemq/conf/activemq.xml
   ```

   配置内容如下，将原来端口**61616**调整成46616

   ```xml
   <transportConnectors>
       <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
       <transportConnector name="openwire" uri="tcp://0.0.0.0:46616?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>
       # 省略
   </transportConnectors>
   ```

5. 配置防火墙

   Web管理平台，默认使用8161端口

   ```shell
   firewall-cmd --zone=public --add-port=8161/tcp --permanent
   ```

   服务端口

   ```shell
   firewall-cmd --zone=public --add-port=46616/tcp --permanent
   ```

   

6. 设置自启动

   创建服务文件

   ```shell
   vi /usr/lib/systemd/system/activemq.service
   ```

   输入以下内容，**注意修改对应路径**

   ```shell
   [Unit]
   Description=ActiveMQ service
   After=network.target
   
   [Service]
   Type=forking
   ExecStart=/usr/local/activemq/bin/activemq start
   ExecStop=/usr/local/activemq/bin/activemq stop
   User=root
   Group=root
   Restart=always
   RestartSec=9
   StandardOutput=syslog
   StandardError=syslog
   SyslogIdentifier=activemq
   
   [Install]
   WantedBy=multi-user.target
   ```

   环境文件补充

   - 开机自启：systemctl enable activemq

   - 查看服务状态：systemctl status activemq

   - 启动activemq服务：systemctl start activemq

   - 停止activemq服务：systemctl stop activemq

     

REF：

[CentOS 7 安装ActiveMQ - Rozdy - 博客园 (cnblogs.com)](https://www.cnblogs.com/Rozdy/p/11497324.html)

[ActiveMQ入门 - CodingDiary - 博客园 (cnblogs.com)](https://www.cnblogs.com/coding-diary/p/12715137.html)
