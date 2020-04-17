# sql优化

## limit 优化

InnoDB中有buffer pool。里面存有最近访问过的数据页，包括数据页和索引页

```
select * from test a inner join (select id from test where val=4 limit 300000,5);
```