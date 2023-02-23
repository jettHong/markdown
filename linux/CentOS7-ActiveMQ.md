# CentOS7 ActiveMQ

0. 下载

     官方地址：[ActiveMQ (apache.org)](https://activemq.apache.org/components/classic/download/)

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

   

2. （可选）运行

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

   

3. 调整启动参数

   （可选）内存配置文件

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

   

4. （可选）调整服务端口

   配置文件

   ```shell
   vi /usr/local/activemq/conf/activemq.xml
   ```

   配置内容如下，将原来端口**61616**调整成**41616**

   ```xml
   <transportConnectors>
       <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
       <transportConnector name="openwire" uri="tcp://0.0.0.0:41616?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>
       # 省略
   </transportConnectors>
   ```

   补充权限验证，在标签broker最后添加

   ```xml
   <plugins>
     <simpleAuthenticationPlugin>
   	<users>
   	<authenticationUser username="admin" password="Syncsoft_3202" groups="users,admins"/>
   	</users>
     </simpleAuthenticationPlugin>
   </plugins>
   ```

   调整web管理界面端口

   修改配置文件<activemq_path>/conf/jetty.xml，找到`id="jettyPort"`项，将value值修改为默认的8161端口以外的其它端口，如48161，`<property name="port" value="48161"/>`。修改完成后保存并重启activemq服务。

   

5. 配置防火墙

   Web管理平台，默认使用48161端口

   ```shell
   firewall-cmd --zone=public --add-port=48161/tcp --permanent
   ```

   服务端口

   ```shell
   firewall-cmd --zone=public --add-port=41616/tcp --permanent
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
