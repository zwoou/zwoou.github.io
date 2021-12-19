---
title: Maven  
date: 2017-08-19 21:31:56   
tags: maven   
categories: 工具  
---

## 安装 ##

1. 下载 [官网](https://maven.apache.org/)
2. 配置Maven
配置环境变量
3. 测试安装
在控制台输入“mvn -v” 获得安装信息
- 官方资料库     [http://mvnrepository.com/](http://mvnrepository.com/)
- 
## 新建项目 ##

idea上使用maven
1. 	maven选项
         webapp勾选
2. Groupid 
	Artifactld 坐标
	Vresion  版本
3. archetypeCatalog=internal 配置文件中加这个
<!--more-->
## 阿里巴巴镜像 ##	
    <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
## 添加资源文件 ##
    <profile>  
    <id>downloadSources</id>  
    <properties>  
        <downloadSources>true</downloadSources>  
        <downloadJavadocs>true</downloadJavadocs>
    </properties>  
    </profile>
    <activeProfiles>  
    <activeProfile>downloadSources</activeProfile>  
    </activeProfiles> 

## 安装jar

安装指定文件到本地仓库命令：mvn install:install-file

-DgroupId=<groupId>       : 设置项目代码的包名(一般用组织名)

-DartifactId=<artifactId> : 设置项目名或模块名 

-Dversion=1.0.0           : 版本号

-Dpackaging=jar           : 什么类型的文件(jar包)
-Dfile=<myfile.jar>       : 指定jar文件路径与文件名(同目录只需文件名)

```shell
mvn install:install-file -DgroupId=com.qrcode -DartifactId=qrcode -Dversion=1.0.0 -Dpackaging=jar -Dfile=qrcode.jar

```

 ## 跳过测试
在<project>标签下的<properties>标签中加入<skipTests>true</skipTests> 

## 依赖范围scope 

- **compile**: 默认范围，用于编译
- **provided**: 类似于编译，但支持你期待jdk或者容器提供，类似于classpath    
- **runtime**: 在执行时需要使用      
- **test**:    用于test任务时使用      
- **system**: 需要外在提供相应的元素。通过systemPath来取得      
- **systemPath**: 仅用于范围为system。提供相应的路径      
- **optional**:   当项目自身被依赖时，标注依赖是否传递。用于连续依赖时使用
