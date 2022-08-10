CentOS7 配置时间同步

```sh
# ===== 安装
yum remove ntp -y
yum install chrony -y

# ===== 配置
cat << EOF > /etc/chrony.conf
# NTP服务源地址
server 10.100.199.250 iburst

# Record the rate at which the system clock gains/losses time.
driftfile /var/lib/chrony/drift

# Allow the system clock to be stepped in the first three updates
# if its offset is larger than 1 second.
makestep 1.0 3

# Enable kernel synchronization of the real-time clock (RTC).
rtcsync

# Enable hardware timestamping on all interfaces that support it.
#hwtimestamp *

# Increase the minimum number of selectable sources required to adjust
# the system clock.
#minsources 2

# 允许本地网络访问？
#allow 192.168.0.0/16

# Serve time even if not synchronized to a time source.
#local stratum 10

# Specify file containing keys for NTP authentication.
#keyfile /etc/chrony.keys

# Specify directory for log files.
logdir /var/log/chrony

# Select which information is logged.
#log measurements statistics tracking
EOF

# ===== 防火墙
firewall-cmd --add-service=ntp --permanent
firewall-cmd --reload

# ===== 配置服务
systemctl enable chronyd.service && systemctl restart chronyd.service && systemctl status chronyd.service

# ===== 设置时区等
timedatectl set-timezone Asia/Shanghai
timedatectl set-ntp yes
timedatectl

# ===== 手工同步
ntpdate 10.100.199.250

```

