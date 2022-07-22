# CentOS 7  安装 NFS 服务与配置使用

0. NFS 介绍

   NFS（Network File System，网络文件系统）是当前主流异构平台共享文件系统之一。

1. 服务组件安装

   ```shell
   # 服务端(共享者)
   yum install -y nfs-utils rpcbind
   
   # 客户端(访问者)
   yum install -y nfs-utils
   ```

2. 服务端配置（10.100.173.200）

   修改权限
   
   ```shell
   chmod 666 /usr/local/nginx/html/static/
   chown -R nfsnobody:nfsnobody /usr/local/nginx/html/static/
   ```
   
   配置共享目录
   
   ```shell
   vim /etc/exports
   #/data/share 10.100.173.200/24(rw,sync,insecure,no_subtree_check,no_root_squash)
   /usr/local/nginx/html/static/ *(rw,sync,all_squash)
   ```
   
   启动服务
   
   ```shell
   # 启动 RPC 服务
   service rpcbind start
   
   # 查看 NFS 服务项
   rpcinfo -p localhost
   
   # 启动 NFS 服务
   service nfs start
   
   # 再次查看 NFS 服务项
   rpcinfo -p localhost
   ```
   
   如果端口变多即证明服务端已启动，如下
   
   ```shell
      program vers proto   port  service
       100000    4   tcp    111  portmapper
       100000    3   tcp    111  portmapper
       100000    2   tcp    111  portmapper
       100000    4   udp    111  portmapper
       100000    3   udp    111  portmapper
       100000    2   udp    111  portmapper
       100005    1   udp  20048  mountd
       100005    1   tcp  20048  mountd
       100005    2   udp  20048  mountd
       100005    2   tcp  20048  mountd
       100005    3   udp  20048  mountd
       100005    3   tcp  20048  mountd
       100003    3   tcp   2049  nfs
       100003    4   tcp   2049  nfs
       100227    3   tcp   2049  nfs_acl
       100003    3   udp   2049  nfs
       100003    4   udp   2049  nfs
       100227    3   udp   2049  nfs_acl
       100021    1   udp  49113  nlockmgr
       100021    3   udp  49113  nlockmgr
       100021    4   udp  49113  nlockmgr
       100021    1   tcp  42210  nlockmgr
       100021    3   tcp  42210  nlockmgr
       100021    4   tcp  42210  nlockmgr
   ```
   
   ```shell
   # 查看目录是否共享正常
   showmount -e localhost
   ```
   
   配置防火墙
   
   ```shell
   # firewall-cmd --zone=public --add-source=10.100.173.202
   
   # 指定IP、指定端口开放（待优化）
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.100.173.201" port protocol="tcp" port="111" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.100.173.201" port protocol="tcp" port="20048" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.100.173.201" port protocol="tcp" port="2049" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.100.173.201" port protocol="tcp" port="49113" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.100.173.201" port protocol="tcp" port="42210" accept"
   
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.100.173.201" port protocol="udp" port="111" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.100.173.201" port protocol="udp" port="20048" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.100.173.201" port protocol="udp" port="2049" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.100.173.201" port protocol="udp" port="49113" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.100.173.201" port protocol="udp" port="42210" accept"
   ```
   
   

3. 客户端设置（10.100.173.201）

   检查服务端可否访问

   ```shell
   showmount -e 10.100.173.200
   
   Export list for 10.100.173.200:
   /usr/local/nginx/html/static *
   ```

   在本地创建目录，需要注意，**本地目录中没有文件及子目录**，本例中本地目录与服务端一致

   ```
   mount 10.100.173.200:/usr/local/nginx/html/static/ /usr/local/nginx/html/static/
   ```

   查看挂载情况

   ```
   df -h | grep 10.100.173.200
   ```

4. 其它说明

   exports参数

   | 参数             | 说明                                                         |
   | :--------------- | :----------------------------------------------------------- |
   | ro               | 只读访问                                                     |
   | rw               | 读写访问                                                     |
   | sync             | 所有数据在请求时写入共享                                     |
   | async            | nfs 在写入数据前可以响应请求                                 |
   | secure           | nfs 通过 1024 以下的安全 TCP/IP 端口发送                     |
   | insecure         | nfs 通过 1024 以上的端口发送                                 |
   | wdelay           | 如果多个用户要写入 nfs 目录，则归组写入（默认）              |
   | no_wdelay        | 如果多个用户要写入 nfs 目录，则立即写入，当使用 async 时，无需此设置 |
   | hide             | 在 nfs 共享目录中不共享其子目录                              |
   | no_hide          | 共享 nfs 目录的子目录                                        |
   | subtree_check    | 如果共享 /usr/bin 之类的子目录时，强制 nfs 检查父目录的权限（默认） |
   | no_subtree_check | 不检查父目录权限                                             |
   | all_squash       | 共享文件的 UID 和 GID 映射匿名用户 anonymous，适合公用目录   |
   | no_all_squash    | 保留共享文件的 UID 和 GID（默认）                            |
   | root_squash      | root 用户的所有请求映射成如 anonymous 用户一样的权限（默认） |
   | no_root_squash   | root 用户具有根目录的完全管理访问权限                        |
   | anonuid=xxx      | 指定 nfs 服务器 /etc/passwd 文件中匿名用户的 UID             |
   | anongid=xxx      | 指定 nfs 服务器 /etc/passwd 文件中匿名用户的 GID             |

REF：

[Linux 环境下 NFS 服务安装及配置使用 - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1433259)

[鸟哥的 Linux 私房菜 -- NFS 服务器 (vbird.org)](http://cn.linux.vbird.org/linux_server/0330nfs_1.php)