---
title: 规范
date: 2019-03-01 08:00:00 
tags: 
    - java
    - 规范
---
## 日志
1. 使用SFL4J中的API
```
   import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
private static final Logger logger = LoggerFactory.getLogger(Abc.class);
``` 

2. 避免重复打印日志，浪费磁盘空间，务必在 log 4 j . xml 中设置 additivity = false 。

## 参考

