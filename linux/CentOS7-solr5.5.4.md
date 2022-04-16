# CentOS7 Solr 5.5.4

0. 下载

   官方地址：[Index of /dist/lucene/solr/5.5.4 (apache.org)](https://archive.apache.org/dist/lucene/solr/5.5.4/)

   ```shell
   # 这里使用项目启用的相应版本（.tgz是Linux版本、.zip是win版本）
   wget https://archive.apache.org/dist/lucene/solr/5.5.4/solr-5.5.4.tgz
   ```
   
1. 解压

   ```shell
   tar -xvf solr-5.5.4.tgz
   ```
   
2. 运行

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

   

4. 设置自启动 TODO

   

5. END

REF：

TODO [SOLR 5.0安装 使用内置脚本安装_Yang---的博客-CSDN博客](https://blog.csdn.net/u011513049/article/details/44887519)

[Centos7下安装Solr服务和Solr基本用法 - 简书 (jianshu.com)](https://www.jianshu.com/p/46af12966ac5)
