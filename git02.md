---
  title: git02
  tags: git
---

# git02

1. 配置代理
git config --global http.proxy http://XXXx.com.cn:8080
git config --global https.proxy https://XXX.com.cn:8080
2. 生成SSH
ssh-keygen -t rsa -C "zwooou@gmail.com"
3. 查看历史
``` shell
git log
```
英文状态下`q`退出
4. 查看远程仓库
``` shell
git remote -v
```
5. 添加远仓库
```
git remote add origin https://gitee.com/zwoou/note.git
```
6. 推送到远程
git push -u origin master
强制推送，会覆盖远程信息
git push -f
7. 用户信息
```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```
7. 拉取 pull=fetch+merge
git pull origin master
8. 删除远程仓库文件
git rm -r --cached '文件路径'
用-r参数删除目录, git rm --cached a.txt 删除的是本地仓库中的文件，且本地工作区的文件会保留且不再与远程仓库发生跟踪关系，如果本地仓库中的文件也要删除则用git rm a.txt
9. 删除远程仓库
git remote rm origin
- 关联
git remote add github git@github.com:michaelliao/learngit.git
10. git warning: LF will be replaced by CRLF in 解决办法 windos 和linux换行符
git config core.autocrlf false
11. ```git revert<commit_id>``` 操作实现以退为进
然后push
git reset 会擦出
git reset --hard commit_id

git rm -r --cached applog\ \/\ 2018-03-29

12. 本地git push 免用户名和密码配置

在终端通过git config --global命令进行配置

```shell
git config --global user.email "xxx@xxmail.com"
git config --global user.name "xxx"
git config --global credential.helper store   # git push的时候记住用户名和密码
git config --global push.default simple       #从Git 2.0之后，push.default的默认值由'matching'改为'simple'
```

实际是改配置文件.gitconfig

```shell
$ cat ~/.gitconfig 
[user]
    email = xxx@xxmail.com
    name = xxx
[credential]
    helper = store
[push]
    default = simple
```
