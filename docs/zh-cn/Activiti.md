---
title: activiti
date: 2017-08-29 18:30:00 
tags: activiti
categories: 工作流
---

# Activiti #
## 概述 ##
- 工作流（Workflow）
- 工作流管理系统（Workflow Management System WfMS）
- 工作流管理联盟（WfMC,Workflow Management Coalition）
- 协议

基于Apache V2协议
<!--more-->
- 下载
[https://www.activiti.org/](https://www.activiti.org/)

- 开发包
解压 activiti-rest项目 里面的lib

## 数据库 ##
- ACT\_RE_*: 'RE'表示repository。 流程定义和流程静态资源（图片，规则，等）
- ACT\_RU_*   'RU'表示runtime, 这些运行时的表，包含流程实例，任务，变量，异步任务，等运行中的数据。 Activiti只在流程实例执行过程中保存这些数据， 在流程结束时就会删除这些记录。 这样运行时表可以一直很小速度很快。			
-  这些运行时的表，包含流程实例，任务，变量，异步任务，等运行中的数据。 Activiti只在流程实例执行过程中保存这些数据， 在流程结束时就会删除这些记录。 这样运行时表可以一直很小速度很快。
-  ACT\_HI_*: 'HI'表示history。 这些表包含历史数据，比如历史流程实例， 变量，任务等等。
-  ACT\_GE_*: 通用数据， 用于不同场景下，如存放资源文件。
### 资源库流程规则表 ###
- 1)	act\_re_deployment 	部署信息表
- 2)	act\_re_model  		流程设计模型部署表
- 3)	act\_re_procdef  		流程定义数据表

### 运行时数据库表 ###

- 1)	act\_ru_execution		运行时流程执行实例表
- 2)	act\_ru_identitylink		运行时流程人员表，主要存储任务节点与参与者的相关信息
- 3)	act\_ru_task			运行时任务节点表
- 4)	act\_ru_variable		运行时流程变量数据表
### 历史数据库表 ###
- 1)	act_hi_actinst 		历史节点表
- 2)	act_hi_attachment		历史附件表
- 3)	act_hi_comment		历史意见表
- 4)	act_hi_identitylink		历史流程人员表
- 5)	act_hi_detail			历史详情表，提供历史变量的查询
- 6)	act_hi_procinst		历史流程实例表
- 7)	act_hi_taskinst		历史任务实例表
- 8)	act_hi_varinst			历史变量表
### 组织机构表 ###
- 1)	act_id_group		用户组信息表
- 2)	act_id_info			用户扩展信息表
- 3)	act_id_membership	用户与用户组对应信息表
- 4)	act_id_user			用户信息表
### 通用数据表 ###
- 1)	act_ge_bytearray		二进制数据表
- 2)	act_ge_property			属性数据表存储整个流程引擎级别的数据,初始化表结构时，会默认插入三条记录，

## 核心API ##
1. ProcessEngine 

	这是Activiti的核心，负责生成流程运行时的各种实例及数据，监控和管理数据的运行
2. RepositoryService 仓库服务类 管理流程定义
3. RuntimeService流程执行服务类，执行管理，包括启动、推进、删除流程实例等操作
4. TaskService 任务管理
5. HistoryService 历史管理（执行完的数据的管理）
6. IdentityService 组织机构管理
7. FormService 任务表单管理，可选
8. managerService 
6. ProcessDefinition 流程定义类,可以从这里获得资源文件等。
7. ProcessInstance 代表流程定义的执行实例
8. Execution 在没有并发时，同ProcessInstance
![](http://i.imgur.com/jKMJu6o.png)
	总结：
	* 一个流程中，执行对象可以存在多个，但是流程实例只能有一个。
	* 当流程按照规则只执行一次的时候，那么流程实例就是执行对象。















