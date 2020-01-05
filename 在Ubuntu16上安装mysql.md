---
title: 在Ubuntu16上安装mysql
tags: 
  - ubuntu
  - mysql
  - linux
---

## 第一步

```shell
sudo apt-get install mysql-server
sudo apt install mysql-client
sudo apt install libmysqlclient-dev
```

安装后测试

```shell
sudo netstat -tap | grep mysql
```

## 第二步，登录

````shell
mysql -u root -p
````

## 第三步，允许远程登录

编辑文件

```shell
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

如果没有，编辑文件

```shell
sudo vim /etc/mysql/my.cnf
```

注释掉bind-address = 127.0.0.1

## 第四步，执行授权

```sql
grant all on *.* to root@'%' identified by '你的密码' with grant option;
flush privileges;
```

然后执行quit命令退出mysql服务

重启mysql

```shell
service mysql restart
```

现在在windows下可以使用navicat远程连接ubuntu下的mysql服务：.

## mysql 启动/停止/重启

1. 启动mysql

   - ```shell
     sudo /etc/init.d/mysql start
     ```

   - ```shell
     sudo start mysql
     ```

   - ```shell
     sudo service mysql start
     ```

     ​

2. 停止mysql

   - ```shell
     sudo /etc/init.d/mysql stop
     ```

   - ```shell
     sudo stop mysql
     ```

   - ```shell
     sudo service mysql stop
     ```

     ​

3. 重启mysql

   - ```shell
     sudo /etc/init.d/mysql restart
     ```

   - ```shell
     sudo restart mysql 
     ```

   - ```shell
     sudo service mysql restart
     ```

     ​