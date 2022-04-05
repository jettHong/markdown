# CentOS7 防火墙（firewall）

0、systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。

启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed

1、基本配合

启动： systemctl start firewalld
查看状态： systemctl status firewalld
禁用，禁止开机启动： systemctl disable firewalld
停止运行： systemctl stop firewalld

2、firewalld-cmd 基本指令

查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
更新防火墙规则，重启服务： firewall-cmd --completely-reload

3、开通端口

```shell
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

--zone=public  名叫public的规则集

--permanent 永久生效，没有此参数重启后失效

