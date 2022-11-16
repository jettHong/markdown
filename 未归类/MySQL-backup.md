# MySQL 备份

问题：mysqldump 备份时有时会出现备份出来的文件为空情况：
错误提示：mysqldump: Got error: 1160: Got an error writing communication packets when using LOCK TABLES


解决方案A：补充 --single-transaction 参数，通过开启事务的方式进行dump。
https://zhuanlan.zhihu.com/p/347105199 mysqldump引发数据库不可读写的血案
https://blog.csdn.net/qq_34556414/article/details/106781973 Mysql备份原理与工具mysqldump



解决方案B：使用 mydumper 替代原始 mysqldump。
https://www.jianshu.com/p/65f33c7eb630 MySQL 工具之mysqldumper介绍
mydumper（http://www.mysqldumper.org/）是一个在GNU GPLv3许可下发布的高性能MySQL备份和恢复工具集。mydumper是多线程的，他创建一个mysql备份就比随mysql发布的mysqldump工具要快得多。

安装
release=$(curl -Ls -o /dev/null -w %{url_effective} https://github.com/mydumper/mydumper/releases/latest | cut -d'/' -f8)
yum install https://github.com/mydumper/mydumper/releases/download/${release}/mydumper-${release:1}.el7.x86_64.rpm

错误提示：Access denied； you need (at least one of) the BACKUP_ADMIN privilege(s) for this operation
解决方法：
grant BACKUP_ADMIN on *.* to 'root'@'%';
FLUSH PRIVILEGES;

备份脚本片段
/usr/bin/mydumper  -h localhost -u root -p 'syncsoft2022_JZ' -B cyfccph -t 4 -r 100000 -c --less-locking  -v 3 -D -L /storage/sql_back_up/log/mydumper.log   -o /storage/sql_back_up

