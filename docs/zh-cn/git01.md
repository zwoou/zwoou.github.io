---
title: git-01
tags: git
---

1. 官网 [https://git-scm.com/](https://git-scm.com/)
2. $ git config --global user.name "Your Name"
3. $ git config --global user.email "email@example.com"
## 创建仓库 ##
1. 创建文件夹
2. git init 初始化仓库
3. git add 文件名
4. git commit -m "提交的说明"
5. git status 查看仓库状态
6. git diff 查看difference
7. git log 查看历史记录
## 版本回退 ##
8. git reset --hard HEAD^ 上一个版本 HEAD^^上上版本 HEAD~100 上一百版本
9. git reflog 记录每一次命令
10. git reset --hard 版本id    穿梭任何历史版本
## 工作区和暂存区 ##
1. 工作区（Working Directory）
版本库（Repository）
## 添加远程仓库 ##
 1. git remote add origin git@github.com:zwoou/learngit.git
 2. git push origin master 本地提交到远程仓库
 ## 创建与合并分支 ##
1. git branch 查看分支
2. git branch <name> 创建分支
3. git checkout <name> 切换分支
4. git checkout -b <name> 创建+切换分支
5. git merge <name> 合并某分支到当前分支
6. git branch -d <name> 删除分支
## 解决冲突 ##
1. git log --graph 分支合并图
2. git merge --no-ff -m "xxx" dev 合并分支 合并后的历史有分支
## BUG分支 ##
1. 首先 git stash
2. 修复bug
3. git stash pop 回到工作现场
## Feature分支 ##
1. git branch -D xxx  强行删除分支
## 多人协作 ##
1. git remote -v 查看远程库信息
2. 本地新建的分支如果不推送，对其他人就是不可见的
3. git push origin branch-name 从本地推送分支,如果失败，先用 git pull 抓取远程的新提交
4. 在本地创建和远程分支对应的分支，使用 git checkout -b branch-name origin/branch-name,本地和远程的分支最好一致。
5. 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name
6. 从远程抓取分支，使用git pull ,如果有冲突，要先处理冲突。
## github ##
1. 克隆 git clone git@github.com:zwoou/bootstrap.git
2. 推送pull request给官方仓库
## 忽略特殊文件 ##
1. git工作区根目录下创建.gitignore文件，然后把忽略的文件名填进去
2. github 已经准备好了各种配置文件[https://github.com/github/gitignore](https://github.com/github/gitignore)
3. 忽略文件的原则：
	1. 忽略操作系统自动生成的文件
	2. 忽略编译生成的中间文件、可执行文件
	3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件
## 远程库 ##
- 第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

$ ssh-keygen -t rsa -C "youremail@example.com"

- 第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
