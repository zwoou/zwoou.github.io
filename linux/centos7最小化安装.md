---
title: centos7最小化安装
tags:
    - centos7
---

## 配置网络

使用命令 `ip addr` 查看网卡信息

安装 `yum install -y net-tools`

## 关闭自带防火墙

停止Firewall
`systemctl stop firewalld`
关闭firewalld自动启动

```shell
systemctl disable firewalld.service 
安装IPtables防火墙 
yum install -y iptables-services
```

## 关闭SELINUX

```shell
vi /etc/selinux/config 
#注释掉下面两行 
#SELINUX=enforcing 
#SELINUXTYPE=targeted 
#增加一行 
SELINUX=disabled
```
保存，关闭,重启

## 安装wget

```shell
yum install -y wget
```

## 更改源

## 安装vim

```shell
yum install -y vim-enhanced
```
