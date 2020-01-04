---
title: tomcat
tags:
    - tomcat
    - 服务器
---
## 端口被占用
两个命令：
```
netstat -ano | findstr 8080

taskkill -pid 进程pid -f
```