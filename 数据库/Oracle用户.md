---
title: oracle用户   
date: 2017-10-17 21:26:42   
tags: 
	- oracle
---
## 1. 用户 ##
  系统用户
   sys,system
	system/12345678Qaz
	connect sys/12345678Qaz as sysdba
   sysman
   scott用户  密码： tiger
	启用用户的语句
		alter user username account unlock;
   查看登陆用户  show user
            desc dba_users数据字典
表空间
 永久表空间
 临时表空间
 UNDO表空间 被修改之前数据
  dba_tablespaces、user_tablespaces数据字典

表与约束
查询语句
	查询下一个序列
	select emp_seq.nextval from dual
查询编码方式
select userenv ('language') from dual; 
- 12c创建创建用户
	create user c##xiaoming identified by xiaoming;
	grant connect,resource to c##xiaoming;
	alter user c##xiaoming quota unlimited on "USERS";
创建序列
  create serquence hibernate_seq(序列名);        
  
 分组
 select cv.*,rank() over(partition by cv.dispose_id order by cv.create_date) rank from Claim_Voucher cv where cv.status=1
