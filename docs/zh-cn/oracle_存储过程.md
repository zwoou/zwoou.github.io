---
title: Oracle.   
date: 2017-10-31 22:07:12   
tags: 
    - oracle
    - 存储过程
---

## 1. 存储过程 ##

1. 基本语法
-- 打开输出
set SERVEROUTPUT ON;
declare
    --说明部分（变量常量说明，光标申明，例外说明）
begin
    --程序(DML语句）
    dbms_output.put_line('Hello World');
exception
    --例外处理语句
end;
/
2. 变量说明
var1 char(15);
married boolean :=true;
psal number(7,2);
my_name emp.ename%type; -- 引用型变量
emp_rec emp%rowtype; --记录型变量
3. if语句
```sql
--接收键盘输入
--num: 地址值，在该地址上保存了输入的值
accept num prompt '请输入一个数字'
declare
  pnum number := &num;
begin
if pnum=0 then  dbms_output.put_line(pnum);
elsif pnum=1 then dbms_output.put_line(pnum);
else dbms_output.put_line(pnum);
end if;
end ;
/
```
4. 循环语句
Loop
exit[when 条件]；
---
End loop; 
```
--打印1-10
declare
 pnum number :=1;
begin
 loop
 exit when pnum >10;
 dbms_output.put_line(pnum);
 pnum := pnum + 1;
 end loop;
end;
```
5. 光标（Cursor）
语法：
cursor 光标名 [（参数名 数据类型[,参数名 数据类型]...)]
    is select 语句;
eg:
    cursor c1 is select ename from emp;
打开光标: open C1;
取一行光标的值： fetch c1 into pename;（取一行到变量中）
关闭光标： close c1;
    1. 光标的属性
%isopen %rowcount(影响的行数）
%notfound %found
