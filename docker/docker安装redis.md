# docker安装redis



## 1. 安装Redis

通过docker search redis 和docker pull redis 下载redis 镜像

## 2. 新建挂载配置文件夹

新键data和conf两个文件夹.

```
mkdir -p /root/docker/redis/data
mkdir -p /root/docker/redis/conf
```

## 3. 增加配置文件 redis.conf

在刚才新建的`redis/conf`中新建文件`redis.conf`，内容如下：

```
#bind 127.0.0.1 //允许远程连接
protected-mode no
appendonly yes //持久化
requirepass 123456 //密码 
```

## 4. 创建`redis`容器并启动

```shell
docker run --name my_redis --privileged=true -p 6379:6379 -v /root/docker/redis/data:/data -v /root/docker/redis/conf/redis.conf:/etc/redis/redis.conf -d redis redis-server /etc/redis/redis.conf 
```

释义如下：

- –name：给容器起一个名
- -p：端口映射 宿主机:容器
- -v：挂载自定义配置 自定义配置:容器内部配置
- -d：后台运行
- redis-server --appendonly yes： 在容器执行redis-server启动命令，并打开redis持久化配置