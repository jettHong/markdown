# MySQL 主从复制

## 原理

主库写入 binlog，从库读取成 relay log 并应用。

同步方式：同步、异步、半同步。同步方向：单向1对1同步、1对多同步。

[复制格式](https://dev.mysql.com/doc/refman/8.0/en/replication-formats.html)：

| 格式                   | 优点               | 缺点                                                    |
| ---------------------- | ------------------ | ------------------------------------------------------- |
| 基于语句的复制（SBR）  | 占用空间小、传输快 | 若有类似环境相关的语句，可能造成误差<br />如  sysdate() |
| 基于行（RBR）          | 完整语句           | 占用空间大、传输慢。                                    |
| 基于混合的复制 （MBR） |                    |                                                         |



![A transaction received by the source is executed, written to the binary log, then committed, and a response is sent to the client application. The record from the binary log is sent to the relay logs on Replica 1 and Replica 2 before the commit takes place on the source. On each of the replicas, the transaction is applied, written to the replica's binary log, and committed. The commit on the source and the commits on the replicas are all independent and asynchronous.](MySQL 主从复制.assets/async-replication-diagram-16317014859972.png)

图源于：[MySQL * MySQL 8.0 参考手册 *： 18.1.1 复制源](https://dev.mysql.com/doc/refman/8.0/en/group-replication-primary-secondary-replication.html)

## 操作步骤

### master 主服务器配置

查询主库配置文件

```mysql
select @@datadir;
```

编辑 my.cnf，找到 [mysqld] 一节，在节点下修改添加。

```bash
[mysqld]

# 设置服务器唯一的id，默认是1，可以根据设置ip最后一段来设置
server-id=1

# 启用二进制日志，这个文件名可随意
log-bin=mysql-bin

# 忽略写入binlog的库，(A)
#binlog-ignore-db = db_ignore_01

# 指定待记录数据库，多个添加多行 （B）
binlog-do-db=db01
binlog-do-db=db02

# 选项A、选项B只需要指定一项。
```

 SQL 窗口添加用户及授权

```mysql
-- 删除用户
-- DROP USER slave;

-- 创建用户 
create user 'slave'@'%' identified with mysql_native_password by 'slave123456';
-- 授权
grant replication slave on *.* to 'slave'@'%' ;
--  identified with mysql_native_password by 'slave123456'; -- mysql 8.0.26 提示异常
-- 确认权限
SHOW GRANTS FOR 'slave'@'%';
select u.host, u.user, u.Repl_slave_priv, u.* from mysql.user u where u.user = 'slave';

-- 查看主库信息，给与从库配置
show master status;
```

重启主库！！！

### slave 从库配置

编辑 my.cnf，找到 [mysqld] 一节，在节点下修改添加。

```bash
[mysqld]

# 设置服务器唯一的id，注意主从库不能相同！！！
server-id=999

# 指定读取数据库
# replicate-do-db=cyfcportal
```

 SQL 窗口添加主库相关信息

```mysql
-- 在从库配置主表信息，
change master to
master_host='59.61.83.130',
master_port=30308,
master_user='root',
master_password='root',
master_log_file='WIN-RIFCSR4LSCR-bin.000050',
master_log_pos=37633;

-- 启动从库进程
start slave;

-- 查看进程列表
show processlist;

-- 查看从库状态，主要看 Slave_IO_Running、Slave_SQL_Running 两个状态皆要为 Yes
show slave status;

-- 删除从库配置
-- stop slave;
-- reset slave all;
```

###  验证

在主库更新删除记录，从库同步更新即正常。



REF：[MySQL 8 复制（一）——异步复制_wzy0623的专栏-CSDN博客_mysql 异步复制](https://wxy0327.blog.csdn.net/article/details/90081518)