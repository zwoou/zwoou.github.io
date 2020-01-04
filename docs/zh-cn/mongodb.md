---
title: mongodb使用
tags:
    - mongodb
---
# mongodb使用

mongodb 简单使用

## 下载

官网：[https://www.mongodb.com](https://www.mongodb.com)

1. 配置环境变量bin 到path中
2. 创建保存数据库目录并启动服务

```bash
mongod --dbpath=d:\mongodb\test
```

## 简单测试

打开客户端，输入mongo即可打开服务

- 显示所有数据库

```bash
>show dbs
```

- 切换到test数据库

```bash
>use test
```

- 创建名为person的集合

```bash
>db.person.insert({"name":"jack","age":25})
```

- 更改

```bash
>db.person.update({"name":"jack","age":50})
```

- 查询集合

```bash
>db.person.find()
```

- 显示所有集合

```bash
>show colections
```

- 删除集合person名称为jack的记录

```bash
> db.person.remove({"name":"jack"})
```

- 显示帮助信息

```bash
>help
```

- 退出

```bash
>exit
```

- 创建数据库所有者角色的用户

```bash
db.createUser(
   {
     user: "test",
     pwd: "123456",
     roles: [ { role: "dbOwner", db: "test" } ]
   }
);

```


## ubuntu16.04 安装

 ``` shell
 sudo apt-get install -y mongodb-org 
 ```

## 修改配置文件

```bash
 vim /etc/mongod.conf
```

## 启动和关闭MongoDB

```shell
sudo service mongod start  

sudo service mongod stop

ps aux | grep mongod   # 查看守护进程mongod的运行状态
```

## 可视化工具

Studio 3T [https://robomongo.org/](https://robomongo.org/)

## 参考

- [https://blog.csdn.net/feiyu_may/article/details/82885247](https://blog.csdn.net/feiyu_may/article/details/82885247)
- [https://robomongo.org/](https://robomongo.org/)
