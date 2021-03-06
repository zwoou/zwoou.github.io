---
title: Oracle月度统计
tags: oracle
---

Oracle按月统计语句
``` sql
--创建表 Test
CTEATE TABLE TEST(
ID  NUMBER NOT NULL,
MODIFIEDTIME  DATE NOT NULL
)
--按月统计
SELECT TO_CHAR(T.MODIFIEDTIME,'YYYY-MM') TIME,COUNT(*) COUNT
FROM TEST T  www.2cto.com  
--这里可加查询条件 WHERE TO_CHAR(T.MODIFIEDTIME,'YYYY') = TO_CHAR(SYSDATE,'YYYY')
GROUP BY TO_CHAR(T.MODIFIEDTIME,'YYYY-MM')   --根据月份来分组
ORDER BY TO_CHAR(T.MODIFIEDTIME,'YYYY-MM') ASC NULLS  LAST  --根据月份来排序
```
<!--more-->

Oracle按天统计语句
``` sql
--创建表 Test
CTEATE TABLE TEST(
ID  NUMBER NOT NULL,
MODIFIEDTIME  DATE NOT NULL
)
--按天统计
SELECT TO_CHAR(T.MODIFIEDTIME,'YYYY-MM-DD') TIME,COUNT(*)    COUNT
FROM   TEST  T   www.2cto.com  
--这里可加查询条件 WHERE TO_CHAR(T.MODIFIEDTIME,'YYYY') = TO_CHAR(SYSDATE,'YYYY')
GROUP BY TO_CHAR(T.MODIFIEDTIME,'YYYY-MM-DD') --根据日期来分组
ORDER BY TO_CHAR(T.MODIFIEDTIME,'YYYY-MM-DD') ASC NULLS LAST --根据日期排序
 
--注：MODIFIEDTIME 为 表TEST里的时间字段，时间类型
--以上代码可直接在数据库里运行
--假如表里还有个数量的字段，要按天统计数量，可将COUNT(*)改为SUM(1)函数
```
Oracle按周统计语句
``` sql
--创建表 Test
CTEATE TABLE TEST(
ID  NUMBER NOT NULL,
MODIFIEDTIME  DATE NOT NULL
)
--按周统计
SELECT TO_CHAR(T.MODIFIEDTIME,'YYYY') YEAR,TO_CHAR(T.MODIFIEDTIME,'IW') TIME,COUNT(*) COUNT  www.2cto.com  
FROM TEST T
--这里可加查询条件 WHERE TO_CHAR(T.MODIFIEDTIME,'YYYY') = TO_CHAR(SYSDATE,'YYYY')
GROUP BY TO_CHAR(T.MODIFIEDTIME,'IW'),TO_CHAR(T.MODIFIEDTIME,'YYYY')   --根据周数来分组
ORDER BY TO_CHAR(T.MODIFIEDTIME,'YYYY'),TO_CHAR(T.MODIFIEDTIME,'IW') ASC NULLS  LAST  --根据周数来排序
--注：MODIFIEDTIME 为 表TEST里的时间字段，时间类型
--以上代码可直接在数据库里运行
--假如表里还有个数量的字段，要按天统计数量，可将COUNT(*)改为SUM(1)函数
```