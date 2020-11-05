---
title: ubuntu安装rpm软件包
tags: 
  - linux
  - ubuntu 
---

## 方法一

1. 安装alien

   ```sudo apt-get install alien fakeroot
     sudo apt-get install fakeroot
   ```

2. 将rpm包转成deb包

   ```fakeroot alien XXX.rpm
   fakeroot alien XXX.rpm
   ```

3. 安装

   ```
   sudo dpkg -i  XXX.deb
   ```

   ## 方法二

   1. CODE

      ```shell
      sudo apt-get install alien
      ```

   2. CODE

      ```shell
      alien --scripts XXX.rpm
      ```

      ​

   3. CODE

      ```shell
      sudo dpkg -i package.deb
      ```

      ​