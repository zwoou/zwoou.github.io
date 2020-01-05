---
title: ubuntu上安装、和卸载软件
tags: 
  - linux
  - ubuntu
---

## 安装软件

1. APT方式

   - 普通安装：`apt-get install softname1...;`

   - 修复安装：`apt-get -f install softname1...;`

   - 重新安装：`apt-get --reinstall install softname1..;`

2. dpkg方式​
  1. 普通安装：`dpkg -i package_name.deb

  2. 源码安装：（.tar、tar.gz,……）

    - 解压XX.tar.gz: `tar zxf xx.tar.gz`


## 卸载方法

1. APT方式
   - 移除式卸载：`apt-get remove softname1……；`
   - 清除式卸载，也清除配置文件：`apt-get --purge remove softname1……;` 

## 查询方法

`dpkg -l` 查询安装的软件

