

# docker安装consul



## 1. 获取consul



```
docker pull consul
```



## 2.启动consul



```
docker run --name consul -d -p 8500:8500 consul
```



浏览器打开 http://192.168.99.100:8500