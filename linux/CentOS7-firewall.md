# CentOS7 防火墙（firewall）

1. 开通关闭端口

   ```SH
   # 开通端口
   firewall-cmd --zone=public --add-port=80/tcp --permanent
   # --zone=public  名叫public的规则集
   # --permanent 永久生效，没有此参数重启后失效
   
   # 开通多个端口（不连续）
   firewall-cmd --zone=public --add-port={80/tcp,82/tcp,84/tcp} --permanent
   
   #关闭一组端口（连续）
   firewall-cmd --zone=public --remove-port=30020-30080/tcp --permanent
   
   # 端口信息查看
   firewall-cmd --list-ports --zone=public
   ```

2. 功能开关启停

   ```SH
   # 重启服务！！！
   systemctl restart firewalld
   
   # 更新规则
   firewall-cmd --completely-reload
   ```

3. 其它指令

   ```SH
   firewall-cmd --help
   firewall-cmd --version
   firewall-cmd --state
   ```

4. REF

   [Linux防火墙Firewall基础详细解读 - 开水没味 - 博客园 (cnblogs.com)](https://www.cnblogs.com/yang0059/p/15102662.html)

