# docker使用



## 1. 数据卷(data volumes)

1. 创建数据卷
   `docker volume create -d local test`

   1. 查看/var/lib/docker/volumes路径下
   2. docker volume还支持inspect（查看详细信息）、ls（列出已有数据卷）、prune（清理无用数据卷）、rm（删除数据卷）等

2. 绑定数据卷
   在用docker [container] run命令的时候，可以使用-mount选项来使用数据卷。
   -mount选项支持三种类型的数据卷，包括：

   ❑ volume：普通数据卷，映射到主机/var/lib/docker/volumes路径下；

   ❑ bind：绑定数据卷，映射到主机指定路径下；

   ❑ tmpfs：临时数据卷，只存在于内存中。

## 2. 端口映射

-p 或-P

1. 从外部访问容器应用

   ​	-P(大写的) Docker 会随机映射一个49000~49900的端口到内部容器开放的网络端口

   





## 3. privileged参数



大约在0.6版，privileged被引入docker。
使用该参数，container内的root拥有真正的root权限。
否则，container内的root只是外部的一个普通用户权限。
privileged启动的容器，可以看到很多host上的设备，并且可以执行mount。
甚至允许你在docker容器中启动docker容器。



## 4. 互联机制实现便捷互访

容器的互联linking 能够让多个容器应用进行快速交互。

--link的参数格式为 `--link name:alias` 

其中name是要链接的容器的名字 ，alias 是别名



