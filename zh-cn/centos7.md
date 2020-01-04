---
title: centos7
tags:
    - centos7
---
## centos7安装JDK

1. 查看是否安装有JDK

```shell
rpm -qa | grep java
```

2. 删除openjdk，在终端输入：rpm -e –-nodeps XXXX_openjdk_XXX 。即可删除自带的openjdk。

上面两部可以合成一步

```shell
rpm -e --nodeps `rpm -qa | grep java`
```

3. JDK安装

## centos7安装openjdk

### 系统版本

```shell
cat /etc/redhat-releas
```

### 安装之前先查看一下有无系统自带jdk

```shell

rpm -qa |grep java

rpm -qa |grep jdk

rpm -qa |grep gcj
```

如果有就使用批量卸载命令

```shell
rpm -qa | grep java | xargs rpm -e --nodeps
```

### 直接yum安装1.8.0版本openjdk

```shell
yum install java-1.8.0-openjdk* -y
```

### 查看版本

```shell
 java -version
```



## 设置中文语言

1. 安装中文语言包
先查看系统是否有安装中文语言包  

```shell
locale -a
```

　{语言代号}_{国家代号}.{字符集}
如果没有发现以上几项，则手动安装中文语言包

```shell
yum install kde-l10n-Chinese
```

1. 修改i18n国际化和locale.conf本土化配置文件

修改i18n配置文件

```shell
vim /etc/sysconfig/i18n
```

添加如下两行代码

```
LANG="zh_CN.UTF-8"
LC_ALL="zh_CN.UTF-8"
```
然后 `source    /etc/sysconfig/i18n`

再修改 locale.cnf配置文件
```
　　　　#   vim /etc/locale.conf

              LANG="zh_CN.UTF-8"
```

          #  source   /etc/locale.conf

重启系统

`# reboot`

## 防火墙

CentOS 7.0默认使用的是firewall作为防火墙
关闭firewall：
systemctl stop firewalld.service #停止firewall
systemctl disable firewalld.service #禁止firewall开机启动
firewall-cmd --state #查看默认防火墙状态（关闭后显示notrunning，开启后显示running）
