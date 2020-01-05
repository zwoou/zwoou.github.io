---
title: Spring Boot   
date: 2017-08-20 09:17:23     
tags: 
	- springboot  
categories: spring  
---

## Spring Boot 简介 ##
四个特性：
- Spring Boot Starter: 常用依赖分组整合
- 自动配置
- 命令行接口（CLI）
- Actuator: 添加管理特性
<!--more-->
## idea 开发Spring Boot步骤 ##
1. Spring Initializr:http://start.spring.io
2. 配置文件
    1. 基本包
    ```
    <groupId>com.example</groupId>
	<artifactId>springbootdemo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>springbootdemo</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.6.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>
    <build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
    ```
    2. web
    ```
    <dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
    ```
3. idea 配置jar包，运行
## Spring Boot 的优缺点 ##
- 优点
    - 快速构建项目
    - 对主流框架的无配置集成
    - 项目可独立运行，无依赖外部Servlet容器
    - 提供运行时的应用监控
    - 极大提高开发和部署效率
    - 与云计算的天然集成
- 缺点

## 部署

   1. start.sh
   ```
		#!/bin/sh
		nohup java -jar demo.jar &
   ```
   2. 查找
   ```
	ps -ef |grep java
   ```
   3. kill进程
















































































