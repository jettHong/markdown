### 1. 环境

- ubuntu 20.04
- MySQL 8

### 2. 安装 MySQL

```shell
# 也许需要先更新下
# sudo apt-get update
sudo apt install mysql-server
```

查看是否安装成功

```shell
systemctl status mysql
```

### 3. 设置用户密码

查看安装时随机生成的账户密码

```shell
sudo cat /etc/mysql/debian.cnf
Automatically generated for Debian scripts. DO NOT TOUCH!
[client]
host     = localhost
user     = debian-sys-maint
password = dGl5QpmsQOMabfSA
socket   = /var/run/mysqld/mysqld.sock
[mysql_upgrade]
host     = localhost
user     = debian-sys-maint
password = dGl5QpmsQOMabfSA
socket   = /var/run/mysqld/mysqld.sock
```

### 4. 服务启停

启动服务 

```shell
sudo service mysql start
```

停止服务

```shell
sudo service mysql stop
```

sudo 登录 MySQL

```shell
sudo mysql
```

修改 [用户密码、可远程访问]

```mysql
mysql> use mysql;
mysql> update user set user.host = '%';
Query OK, 5 rows affected (0.00 sec)
Rows matched: 5  Changed: 5  Warnings: 0
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

重启 MySQL 服务

```shell
sudo service mysql restart
```

系统设置，防火墙相关

