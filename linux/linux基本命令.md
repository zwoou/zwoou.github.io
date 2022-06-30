# linux基本命令

## 系统监控

- `top`

## 包管理

### yum命令

yum（ Yellow dog Updater, Modified）是一个在 Fedora 和 RedHat 以及 SUSE 中的 Shell 前端软件包管理器。

基于 RPM 包管理，能够从指定的服务器自动下载 RPM 包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。

yum 提供了查找、安装、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

### yum语法

```shell
yum [options] [command] [package ...]
```
- **options：**可选，选项包括-h（帮助），-y（当安装过程提示选择全部为 "yes"），-q（不显示安装的过程）等等。
- **command：**要进行的操作。
- **package：**安装的包名。

### yum常用命令

- 1. 列出所有可更新的软件清单命令：**yum check-update**

- 2. 更新所有软件命令：**yum update**

- 3. 仅安装指定的软件命令：**yum install <package_name>**

- 4. 仅更新指定的软件命令：**yum update <package_name>**

- 5. 列出所有可安裝的软件清单命令：**yum list**

- 6. 删除软件包命令：**yum remove <package_name>**

- 7. 查找软件包命令：**yum search <keyword>**

- 8. 清除缓存命令:

  - **yum clean packages**: 清除缓存目录下的软件包
  - **yum clean headers**: 清除缓存目录下的 headers
  - **yum clean oldheaders**: 清除缓存目录下旧的 headers
  - **yum clean, yum clean all (= yum clean packages; yum clean oldheaders)** :清除缓存目录下的软件包及旧的 headers
  
  ## 常用系统命令
  
  ### echo命令
  
  在终端输出字符串值或者变量
  
  ### date命令
  
  显示及设置系统的时间或日期 格式 date [选项] [+指定的格式]
  
  | 参数 | 作用         |
  | ---- | ------------ |
  | %t   | 跳格（Tab)   |
  | %H   | 小时（0-24） |
  | %I   | 小时（0-12） |
  | %M   | 分钟         |
  
  ### reboot命令
  
  重启系统
  
  ### poweroff命令
  
  关闭系统
  
  ### wget命令
  
  -b 后台下载模式
  
  -P 下载到指定目录
  
  -t   最大尝试次数
  
  -c  断点续传
  
  -p 下载页面内所有资源
  
  -r 递归下载
  
  ### ps命令
  
  查看系统中的进程状态
  
  | 参数 | 作用                   |
  | ---- | ---------------------- |
  | -a   | 显示所有的进程         |
  | -u   | 用户以及其他详细信息   |
  | -x   | 显示没有控制终端的进程 |
  
  ## top命令
  
  ## kill命令
  
  ## 系统状态检测命令
  
  ## history 显示历史命令
  
  !+数字 执行历史命令
  