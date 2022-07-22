# Centos 7 yum源配置

### 方法一：添加CD源（挂载 ISO 镜像）

挂载光盘

```shell
mkdir /media/cdrom
mount /dev/cdrom /media/cdrom
```

添加 yum 源配置文件，本例中已有配置

```shell
vi /etc/yum.repos.d/CentOS-Media.repo
```

添加如下内容

```ini
[c7-media]
name=CentOS-$releasever - Media
baseurl=file:///media/CentOS/
        file:///media/cdrom/
        file:///media/cdrecorder/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

### 方法二、添加163源

使用说明

首先备份/etc/yum.repos.d/CentOS-Base.repo

```shell
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

下载对应版本repo文件, 放入/etc/yum.repos.d/(操作前请做好相应备份)

- [CentOS7](http://mirrors.163.com/.help/CentOS7-Base-163.repo)
- [CentOS6](http://mirrors.163.com/.help/CentOS6-Base-163.repo)
- [CentOS5](http://mirrors.163.com/.help/CentOS5-Base-163.repo)

```shell
cd /etc/yum.repos.d/
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo
```

运行以下命令生成缓存

```shell
yum clean all
yum makecache
```

