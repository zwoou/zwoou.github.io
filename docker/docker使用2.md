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