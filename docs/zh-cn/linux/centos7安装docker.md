---
title: centos7安装docker
tags:
    - centos7
---







## 查看系统版本

` uname -r ` Docker 要求 CentOS 系统的内核版本高于 3.10

## 移除旧版本

```shell
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

## 安装

添加必要工具

```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

添加软件源信息：

```shell
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

更新 yum 缓存：





```shell
sudo yum makecache fast
```

安装 Docker-ce：

```shell
sudo yum -y install docker-ce
```

启动 Docker 后台服务

```shell
sudo systemctl start docker
```

设置开机启动

```shell
systemctl enable docker
```

测试运行 hello-world

```shell

## 删除 Docker CE

​```shell
sudo yum remove docker-ce
sudo rm -rf /var/lib/docker

```

## centos7查看开机启动项

```shell
systemctl list-unit-files
```

左边是服务名称，右边是状态，enabled是开机启动，disabled是开机不启动

过滤查询可以systemctl list-unit-files | grep enable 过滤查看启动项如下

过滤查询可以systemctl list-unit-files | grep zabbix 过滤查看某服务名如下



## 设置镜像

配置阿里镜像加速

1. 找到**容器镜像服务**

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
"registry-mirrors": ["https://tkiuxugr.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```












