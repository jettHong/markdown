# CentOS7 Solr 5.5.4

0. 下载

    官方地址：[Index of /dist/lucene/solr/5.5.4 (apache.org)](https://archive.apache.org/dist/lucene/solr/5.5.4/)

    ```shell
    cd /usr/local/
    # 这里使用项目启用的相应版本（.tgz是Linux版本、.zip是win版本）
    wget https://archive.apache.org/dist/lucene/solr/5.5.4/solr-5.5.4.tgz
    ```

1. 解压

   ```shell
   tar -xvf solr-5.5.4.tgz
   ```

2. 运行（需要配置成服务时，请跳过此步骤）

   ```shell
   # 进入 activemq/bin 目录
   cd /usr/local/solr-5.5.4/bin
   
   # 运行程序
   ./solr start
   
   # 检查进程
   ps -ef|grep solr
   
   # 停止
   ./solr stop
   ```

3. 配置防火墙

   ```bash
   firewall-cmd --zone=public --add-port=8983/tcp --permanent
   ```

4. 设置自启动

   创建应用程序和数据目录，注意下面的两个目录注意变更

   ```sh
   # 第一个目录是solrhome目录，第二个是启动目录
   mkdir -p /data/solr /usr/local/solr
   ```

   创建用户与变更权限

   ```sh
   groupadd solr
   useradd -g solr solr
   # 目录与上面一致
   chown -R solr.solr /data/solr /usr/local/solr
   ```

   安装服务

   ```sh
   /usr/local/solr-5.5.4/bin/install_solr_service.sh /usr/local/solr-5.5.4.tgz -d /data/solr -i /usr/local/solr
   ```

5. 添加IK

   复制 ik-analyzer-solr5-5.x.jar 到 `/usr/local/solr/solr/server/solr-webapp/webapp/WEB-INF/lib`目录下。

   重新设置权限

   ```sh
   chown -R solr.solr /data/solr /usr/local/solr 
   ```

6. 添加core

   ```sh
   # 方法一 当前测试有问题
   # /usr/local/solr/solr/bin/solr create -c content_solr
   
   # 方法二
   # 创建目录
   mkdir /data/solr/data/content_solr
   # 复制配置文件
   cp /usr/local/solr/solr-5.5.4/server/solr/configsets/sample_techproducts_configs/* /data/solr/data/content_solr/ -Rf
   ```

   手工替换业务指定的 managed-schema 到目录 `/data/solr/data/content_solr/conf/`

   ```sh
   # 重新设置权限
   chown -R solr.solr /data/solr /usr/local/solr 
   # 重启服务
   service solr stop && service solr start && service solr status
   ```

   在 web 管理界面 http://127.0.0.1:8983/solr/#/ 点击左侧 Core Admin ，Add Core 弹窗口中 name、instanceDir 都写入例子中的业务名 `content_solr`

7. EOF

REF：

[SOLR 5.0安装 使用内置脚本安装_Yang---的博客-CSDN博客](https://blog.csdn.net/u011513049/article/details/44887519)

[Centos7下安装Solr服务和Solr基本用法 - 简书 (jianshu.com)](https://www.jianshu.com/p/46af12966ac5)

[solr5.5.4直接部署Linux环境下 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/30734525)
