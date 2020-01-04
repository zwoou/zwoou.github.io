---
title: Oracle  
tags: oracle  
---

# PLSQL #
	Procedure Language/SQL
## SQL优点 ##
- 交互式非过程化
- 数据操纵功能强
- 自动导航语句简单
- 调试容易使用方便

<!--more-->

## 语法格式 ##
1. 块结构示意图
pl/sql块由三个部分构成：定义部分，执行部分，例外处理部分。
如下所示：
declare
/*定义部分——定义常量、变量、游标、例外、复杂数据类型*/
begin
/*执行部分——要执行的pl/sql语句和sql语句*/
exception
/*例外处理部分——处理运行的各种错误*/
end;
定义部分是从declare开始的，该部分是可选的；
执行部分是从begin开始的，该部分是必须的，至少要写null，不能不写；
例外处理部分是从exception开始的，该部分是可选的。

set serveroutput on 打开输出语句
/ 执行上一个命令

	declare
		声明常量 ,必须为其赋值
			c_rate constant number(3,2):=0.10;
		声明变量
         变量名   类型  赋值：=  键盘输入&
				引用类型 %type  引用型变量
						%rowtype 记录型变量
	begin --程序
		into 是动态赋值
	   select c.* into v_ll from claim_voucher c where c.claid=v_id;
		输出语句
			dbms_output.put_line( 字符串加||        );

	end;
## 数据类型 ##
- 标量类型  ： 数字、字符、布尔型、日期时间、参照类型
- LOB类型：BFILE、blob、clob、nclob
## 流程控制结构 ##
     if Conditions
      then 
       CodeExcution
     end if;
    if Conditions 
      then
      CodeExecution
      else CodeExecution
    end if;
    if Conditions 
      then
      CodeExecution
      else if Conditions 
    then
      CodeExecution
    else CodeExecutio
      end if;
    end if;
## 函数 ##
### 字符串函数 ###
	lower()  将所有的字符小写
	upper() 将所有的字符大写
	length()
	replace('','','')替换
	substr(string,start,count)截取
	initcap 第一个字母变大写
### 数值函数 ###
	round() 四舍五入

	trunc(数值，精度) 舍掉 

### 日期函数 ###
add_months()  增加或减去月份
	SQL> select to_char(add_months(to_date(’199912’,’yyyymm’),2),’yyyymm’) from dual; 
last_day() 返回日期的最后一天
sysdate 获取当前的系统日期
extract() 抽取日期的一部分
	select extract(day from sysdate) from dual;
trunc()


	
