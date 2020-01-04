--- 
title: curl使用
tags: 
    - curl
catepories:
    - 工具
---

# 官网

- [https://curl.haxx.se/](https://curl.haxx.se/)

## 1. get

```shell
$curl http://www.yahoo.com/login.cgi?user=nickname&password=12345
```

## 2. post

```shell
$curl -d "user=nickname&password=12345" http://www.yahoo.com/login.cgi
```

## 

 curl --dump-header header 'host: www.hystrix.com' http://localhost:8080/delay/3

