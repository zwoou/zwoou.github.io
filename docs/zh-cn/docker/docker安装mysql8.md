---
title: docker安装mysql8
tags:
	- docker
---

## 拉取镜像

```
docker pull mysql:8.0
```

## 创建并启动容器

 -p: 映射本地端口3306

​        --restart-always: docker服务启动时，自动启动容器，并且当容器停止时，尝试重启容器。

​                --restart具体参数值详细信息：
​                no -  容器退出时，不重启容器；
​                on-failure - 只有在非0状态退出时才从新启动容器；
​                always - 无论退出状态是如何，都重启容器；

​        MYSQL_ROOT_PASSWORD：设置root密码为root

设置默认数据库编码为utf8mb4,默认排序规则为utf8mb4_unicode_ci

​        -v : 挂载本地卷

 注意：mysql8.0安装默认编码为utf8mb4，所以可以不需要参数--character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
docker run \
--name mysql \
-p 3306:3306 \
--restart=always \
-e MYSQL_ROOT_PASSWORD=root \
-v /var/lib/mysql/:/var/lib/mysql/ \
-d mysql:8.0 \
--character-set-server=utf8mb4 \
--collation-server=utf8mb4_unicode_ci

## 进入docker的mysql容器

```
docker exec -it mysql bash
 mysql -uroot -p
 mysql> use mysql; 
 mysql>  ALTER user 'root'@'%' IDENTIFIED BY 'root1234';
 mysql> flush privileges;
```

修改root用户插件验证方式：

```
mysql>  ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';
```

注意:先更改ser表中用户为root的host字段，若为localhost则改为%，只有改为%，该用户才可以远程访问。

此时，可以使用mysql客户端工具连接数据库。

如果navicat 提示“1045 access denied for user 'root'@'localhost' ”，则执行：

mysql> alter user 'root'@'localhost' identified by 'root';

同理：如果navicat 提示“1045 access denied for user 'root'@'%' ”，则执行：

mysql> alter user 'root'@'%' identified by 'root';

刷新权限：

mysql> flush privileges;

\9. 修改mysql数据库编码，防止中文乱码

​    （1）进入docker的mysql容器
​            \#  docker exec -it mysql /bin/bash

​    （2）容器默认没有安装任何编辑器，先安装vim。

​            \#  apt-get update && apt-get install vim -y

​      (3)  安装完vim之后，开始修改mysql数据库编码

​            \#  vim /etc/mysql/conf.d/mysql.cnf

​            增加以下内容，然后保存，退出：     

​          [client]
​            default-character-set=utf8

​          [mysql]
​            default-character-set=utf8

​      (4)  重启mysql容器，查询编码：此时编码已经修改为utf8。

​            mysql>  show variables like'character%';

\10.  查看挂载卷位置：获取容器/镜像的元数据。

​      \#  docker inspect  容器ID

其中：
    "Mounts": [
            {
                "Type": "volume",
                "Name": "ebc0e8f50d451650f29d7ac1a696a0130316073173835c1b5c9f7f88c5fb976f",
                "Source": "/var/lib/docker/volumes/ebc0e8f50d451650f29d7ac1a696a0130316073173835c1b5c9f7/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ]

Source：为本地主机挂载卷的路径