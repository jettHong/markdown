# RocketMQ在windows环境下的安装与配置

官网：[Apache RocketMQ](http://rocketmq.apache.org/)

源码页面：[GitHub - apache/rocketmq: Mirror of Apache RocketMQ](https://github.com/apache/rocketmq)

## 1.  下载RocketMQ

   [Downloading the Apache RocketMQ Releases - Apache RocketMQ](http://rocketmq.apache.org/dowloading/releases/)

## 2. 配置运行

   REF：[Quick Start - Apache RocketMQ](http://rocketmq.apache.org/docs/quick-start/)

   Windows环境下：

   配置环境变量

   ```sh
   ROCKETMQ_HOME="D:\rocketmq"
   NAMESRV_ADDR="localhost:9876"
   ```

   Start Name Server

   ```sh
   .\bin\mqnamesrv.cmd
   ```

   Start Broker

   ```sh
   .\bin\mqbroker.cmd -n localhost:9876 autoCreateTopicEnable=true
   ```

   单机测试可修改内存配置

   如打开 runbroker.* ，搜索 `Xms` 定位内存配置，直接修改 `-Xms512m -Xmx512m`

## 3. 查看

~~旧项目：https://github.com/apache/rocketmq-externals/ rocketmq-console~~

RocketMQ Dashboard 源码：[GitHub - apache/rocketmq-dashboard](https://github.com/apache/rocketmq-dashboard)

下载源码，修改配置

rocketmq-dashboard\src\main\resources\application.properties

```yaml
# 服务IP
server.address=0.0.0.0
# 服务端口
server.port=8080
# namesrvAddr配置：1、使用环境变量 rocketmq.config.namesrvAddr  NAMESRV_ADDR | 2、自定义配置 localhost:9876
rocketmq.config.namesrvAddr=localhost:9876
```

进入源码目录： `mvn spring-boot:run` 

浏览器打开 http://localhost:8080/#/

切换标签，没有错误提示即正常。

REF：[RocketMQ使用文档 / UserGuide_CN.md](https://github.com/apache/rocketmq-dashboard/blob/master/docs/1_0_0/UserGuide_CN.md)

## 4. 验证功能



## 概念特性、架构设计、样例、最佳实践、运维管理

[rocketmq/docs/cn at master · apache/rocketmq · GitHub](https://github.com/apache/rocketmq/tree/master/docs/cn)