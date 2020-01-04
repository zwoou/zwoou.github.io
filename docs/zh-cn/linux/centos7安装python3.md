---
title: centos7安装python3
tags:
    - centos7
---

# centos7 安装 python3.7

## 查看版本

```shell
python
```

## 查看位置

```shell
which python
```

一般位于/usr/bin/python目录下。

## 首先安装依赖

```shell
yum -y groupinstall "Development tools"
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
yum install libffi-devel -y

```

## 创建安装目录

我放在/usr/local/python3,使用命令

```shell
mkdir /usr/local/python3
```

然后下载文件

```shell
wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz
```

进入该目录解压

```shell
tar -vxzf Python-3.7.4.tgz
cd Python-3.7.4
./configure --prefix=/usr/local/python3
make && make install
```

## 最后创建软链接

```shell
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```

## 在命令行中输入python3测试

```shell
python3
```