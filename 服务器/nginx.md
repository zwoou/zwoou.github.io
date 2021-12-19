# nginx

## 下载

- [http://nginx.org/en/linux_packages.html#Ubuntu](http://nginx.org/en/linux_packages.html#Ubuntu)

## 安装

```shell
sudo apt update
sudo apt install nginx
```

或者

```bash
# 安装依赖
yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel
# 解压缩
tar -zxvf linux-nginx-1.12.2.tar.gz
cd nginx-1.12.2/
# 执行配置
./configure
# 编译安装(默认安装在/usr/local/nginx)
make
make install
```

## 常用命令

```shell
nginx #启动
nginx -s stop #快速停止
nginx -s quit #停止服务器,但要等到请求处理完毕后关闭 。
nginx -s reload #重新加载配置文件。
```

## 配置

1. 在http节点下，添加upstream节点。

```txt
upstream linuxidc {
      server 10.0.6.108:7080;
      server 10.0.0.85:8980;
}
```

2. 将server节点下的location节点中的proxy_pass配置为：http:// + upstream名称.

```
location / {
            root  html;
            index  index.html index.htm;
            proxy_pass http://linuxidc;
}
```

3.  现在负载均衡初步完成了。upstream按照轮询（默认）方式进行负载，每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能 自动剔除。虽然这种方式简便、成本低廉。但缺点是：可靠性低和负载分配不均衡。适用于图片服务器集群和纯静态页面服务器集群。

  除此之外，upstream还有其它的分配策略，分别如下：

    - weight（权重）

指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。如下所示，10.0.0.88的访问比率要比10.0.0.77的访问比率高一倍。

```
upstream linuxidc{
      server 10.0.0.77 weight=5;
      server 10.0.0.88 weight=10;
}
```

    - ip_hash（访问ip）
每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。

```
upstream favresin{
      ip_hash;
      server 10.0.0.10:8080;
      server 10.0.0.11:8080;
}
```

    -  fair（第三方）
    
    按后端服务器的响应时间来分配请求，响应时间短的优先分配。与weight分配策略类似。

```
 upstream favresin{     
      server 10.0.0.10:8080;
      server 10.0.0.11:8080;
      fair;
}
```

    - url_hash（第三方）

upstream还可以为每个设备设置状态值，这些状态值的含义分别如下：

**down** 表示单前的server暂时不参与负载.

**weight** 默认为1.weight越大，负载的权重就越大。

**max_fails**：允许请求失败的次数默认为1.当超过最大次数时，返回proxy_next_upstream 模块定义的错误.

**fail_timeout**: max_fails次失败后，暂停的时间。

**backup**： 其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。

```
upstream bakend{ #定义负载均衡设备的Ip及设备状态
      ip_hash;
      server 10.0.0.11:9090 down;
      server 10.0.0.11:8080 weight=2;
      server 10.0.0.11:6060;
      server 10.0.0.11:7070 backup;
}
```

## nginx自动检测后台服务器健康状态

1. ngx_http_proxy_module 模块和ngx_http_upstream_module模块（自带）

  严格来说，nginx自带是没有针对负载均衡后端节点的健康检查的，但是可以通过默认自带的ngx_http_proxy_module 模块和ngx_http_upstream_module模块中的相关指令来完成当后端节点出现故障时，自动切换到健康节点来提供访问。
       这里列出这两个模块中相关的指令：
ngx_http_proxy_module 模块中的 proxy_connect_timeout 指令、proxy_read_timeout指令和proxy_next_upstream指令

```
语法: proxy_connect_timeout time;
默认值:    proxy_connect_timeout 60s;
上下文:    http, server, location
```

设置与后端服务器建立连接的超时时间。应该注意这个超时一般不可能大于75秒。

```
语法: proxy_read_timeout time;
默认值:    proxy_read_timeout 60s;
上下文:    http, server, location
```

定义从后端服务器读取响应的超时。此超时是指相邻两次读操作之间的最长时间间隔，而不是整个响应传输完成的最长时间。如果后端服务器在超时时间段内没有传输任何数据，连接将被关闭。

```
语法: proxy_next_upstream error | timeout | invalid_header | http_500 | http_502 | http_503 | http_504 |http_404 | off ...;
默认值:    proxy_next_upstream error timeout;
上下文:    http, server, location
```

指定在何种情况下一个失败的请求应该被发送到下一台后端服务器：

2. ngx_http_upstream_module模块中的server指令

```
语法: server address [parameters];
默认值:    —
上下文:    upstream
```

范例如下：

```
        upstream name {
                server 10.1.1.110:8080 max_fails=1 fail_timeout=10s;
                server 10.1.1.122:8080 max_fails=1 fail_timeout=10s;
        }

```



proxy_connect_timeout   这个参数， 这个参数是连接的超时时间。 我设置成1，表示是1秒后超时会连接到另外一台服务器。

3. nginx_upstream_check_module 模块

淘宝技术团队开发的模块,在官网[http://tengine.taobao.org/](http://tengine.taobao.org/)

为nginx打补丁



