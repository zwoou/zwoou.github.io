---
title: oracle的job
date: 2017-11-07 22:10:31 
tags: 
  - oracle
  - job
---
## 创建存储过程 ##
``` sql
create or replace procedure P_Td_Sample_Info_net_value
as
       cursor c_postype is select info.sample_id from Td_Sample_Info info;
       v_Sample_Id Td_Sample_Info.Sample_Id%type;
       v_Net_Value Td_Sample_Info.Net_Value%type;
       V_Life_Time Td_Sample_Info.Life_Time%type;
       V_days Td_Sample_Info.Life_Time%type;
       V_Create_Date Td_Sample_Info.Create_Date%type;
       V_Cost  Td_Sample_Info.Cost%type;
       V_usedMonth number;
       V_leftMonth number;
begin
  open c_postype;
  loop
  fetch c_postype into v_Sample_Id;
     exit when c_postype%notfound;
         select
               info.net_value,
               info.life_time,
               info.create_date,
               info.cost
               into v_Net_Value,
                    V_Life_Time,
                    V_Create_Date,
                    V_Cost
           from Td_Sample_Info info where info.sample_id = v_Sample_Id;
          -- 计算日期之间相差得
          -- select  months_between(sysdate,V_Create_Date) into V_days from dual;
         if v_Net_Value is not null and v_Net_Value>0 then
           -- 计算日期之间相差得
            select trunc(V_Life_Time - months_between(sysdate,V_Create_Date)) into V_days from dual;

         elsif V_Cost is not null and V_Cost>0 then
            select trunc(V_Life_Time - months_between(sysdate,V_Create_Date)) into V_usedMonth from dual;
            if V_usedMonth < 0 then
               V_usedMonth := 0;
            end if;
            V_leftMonth := V_Life_Time-V_usedMonth;
            if V_leftMonth = V_Life_Time  then
              update Td_Sample_Info info set info.net_value=V_Cost where info.sample_id=v_Sample_Id;
            elsif V_leftMonth<V_Life_Time then
                  if(V_Life_Time <= 0)then
                      update Td_Sample_Info info set info.net_value=0.00 where info.sample_id=v_Sample_Id;
                  else
                      if(V_Cost/V_Life_Time)*V_days>0 then
                          update Td_Sample_Info info set info.net_value=round((V_Cost/V_Life_Time)*V_days,2) where info.sample_id=v_Sample_Id;
                      else
                          update Td_Sample_Info info set info.net_value=0.00 where info.sample_id=v_Sample_Id;
                      end if;
                  end if;

            end if;
         end if;
  end loop;
  close c_postype;
  commit;
  DBMS_output.put_line('存储过程创建成功！');
  end P_Td_Sample_Info_net_value;
```
<!-- more -->
- 测试
```
begin
  P_Td_Sample_Info_net_value;
  end;
```
- 删除存储过程
```
DROP PROCEDURE P_Td_Sample_Info_net_value;
```
## 创建job ##
``` sql
  declare

           job1 number;

    begin

           dbms_job.submit(job1, 'cc_test;',sysdate, 'sysdate + 30/(24 *60 * 60)');    --每30秒插入一条记录

   end;
```
- 三个job表
```
         select * from dba_jobs;

         select * from all_jobs;

         select * from user_jobs;
```
## 运行job ##
```
       begin

               dbms_job.run(1746);      --和select * from user_jobs;中的job值对应，看what对应的过程

       end;
         -- 正在运行job

         select * from dba_jobs_running;
```
## 删除一个job ##
```
     begin

             dbms_job.remove(1746);--和select * fromuser_jobs; 中的job值对应，看what对应的过程

     end;
```