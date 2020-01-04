---
title: windows安装mysql
date: 2019-03-25 18:27:44
tags:
    - mysql
categories:
    - db
---
# windows安装mysql

1. 在 mysql 目录下新建 my.ini 文件
   内容

``` ini

    [mysqld]

    #设置3306端

    port = 3306

    # 设置mysql的安装目录

    basedir=E:\mysql

    # 设置mysql数据库的数据的存放目录

    datadir=E:\mysql\data

    # 允许最大连接数

    max_connections=200

    # 服务端使用的字符集默认为8比特编码的latin1字符集

    character-set-server=utf8

    # 创建新表时将使用的默认存储引擎

    default-storage-engine=INNODB

    sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

    [mysql]

    # 设置mysql客户端默认字符集

    default-character-set=utf8
```

1. 设置bin环境变量

1. 运行安装服务  
   `mysqld --install`  
   如果失败,移除已经安装的  
   `mysqld --remove`  
1. 运行命令(此时会生成data目录)  
`mysqld  --initialize`
1. 启动服务  
`net start mysql`
1. 停止服务  
`net stop mysql`
1. 打开cmd  
   `mysql -u root -p`