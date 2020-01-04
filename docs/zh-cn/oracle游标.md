---
title : oracle 游标
date : 2017-11-26 09:54:52 
tags : 
  - oracle
  - 游标
---
## 1. 游标的类型 ##
**1. 隐式游标：** 在PL/SQL程序中执行DML SQL语句时会自动创建隐式游标，名字固定为sql.
    q在PL/SQL中使用DML语句时自动创建隐式游标 q隐式游标自动声明、打开和关闭，其名为 SQL q通过检查隐式游标的属性可以获得最近执行的DML 语句的信息 q隐式游标的属性有： q%FOUND – SQL 语句影响了一行或多行时为 TRUE q%NOTFOUND – SQL 语句没有影响任何行时为TRUE q%ROWCOUNT – SQL 语句影响的行数 q%ISOPEN - 游标是否打开，始终为FALSE
<!--more-->
```
begin 
  update student s set s.age=s.age + 10;
  if sql %found then
    dbms_output.put_line('这次更新了'|| sql%rowcount);
    else
      dbms_output.put_line('一行也没更新');
  end if;
end;
declare 
 v_name student.name%type;
 begin
   select t.name into v_name  from student t where t.id=2;
   if sql%found then
     dbms_output.put_line(sql%rowcount);
   else
     dbms_output.put_line('没有找到数据');
   end if;
 exception
   when too_many_rows then
     dbms_output.put_line('查找的行记录多于1行');
   when no_data_found then
     dbms_output.put_line('未找到匹配的行');
 end;
```
**2. 显示游标：** 显示游标用于处理返回多行的查询
```
--无参数游标
declare
  cursor c_student is select s.name from student s;
  v_name student.name%type;
begin 
  open c_student; --打开游标
    fetch c_student into v_name;
    while c_student%found 
      loop
        dbms_output.put_line('学生姓名：'||v_name);
      fetch c_student into v_name;  
      end loop;     
  close c_student;
end;  
-- 第二种
declare
  cursor c_student is select s.id, s.name from student s where s.name='ha';
  v_name student.name%type;
  v_id student.id%type;
begin 
  open c_student; --打开游标
    loop
        fetch c_student into v_id,v_name;
        exit when c_student%notfound; 
          dbms_output.put_line('学生姓名：'||v_name);  
    end loop;     
  close c_student;
end;
-- 有参数游标
declare
  cursor c_student(input_v_id number) is select * from student s;
  v_id student.id%type;
  v_name student.name%type;
  v_age student.age%type;
  v_student c_student%rowtype;
begin
  v_id := 1;
  open c_student(v_id);
  loop
    fetch c_student into v_student;
    exit when c_student%notfound;
      dbms_output.put_line(v_student.name||':'||v_student.age); 
  end loop;
  close c_student;
end;
```
**3. REF游标：** REF游标用于处理运行时才能确定的动态SQL查询的结果
qREF 游标和游标变量用于处理运行时动态执行的 SQL 查询 q创建游标变量需要两个步骤： q声明 REF 游标类型 q声明 REF 游标类型的变量 q用于声明 REF 游标类型的语法为：
``` sql
TYPE <ref_cursor_name> IS REF CURSOR;
[RETURN <return_type>];
-- ref游标
declare
  type ref_cursor is ref cursor; -- 声明一个ref游标类型
  tab_cursor ref_cursor; --声明一个ref游标
  v_student student%rowtype;
  tab_name varchar2(20);
begin
  tab_name := '&tab_name';--接受客户输入的表名
  if tab_name='student' then
    open tab_cursor for select * from student s;-- 打开游标
      loop
        fetch tab_cursor into v_student;
        exit when tab_cursor%notfound;
          dbms_output.put_line(v_student.name||':'||v_student.age); 
      end loop;
    close tab_cursor;
  else
    dbms_output.put_line('没有你想要的找的表数据信息');
  end if;
end;
```



























