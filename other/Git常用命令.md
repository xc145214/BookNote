# Git 常用命令

[TOC]

## 新建代码库
```
# 在当前目录创建新的 git 代码库
$ git init

# 新建一个目录并初始化为Git代码库
$ git init [project-name]

# 下载一个项目和整个代码历史

$ git clone [url]
```

## 配置
```
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"

# 记住用户密码
$ git config credential.helper store
```

## 增加/删除文件

```
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

## 代码提交

```
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

## 分支

```
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支 
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge --no-ff [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]

# 删除分支后刷新对应关系
$ git remote show origin  
$ git remote prune origin

# no branch 时处理
$ git reflog show
```

## 管理远程分支

```
# 推送到默认远程分支
$ git push origin master

# 创建新分支并推送-
$ git push origin develop

# 拉取服务器特定分支
$ git checkout --track origin/develop

# 同步本地远程分支
$ git fetch origin

# 提交分支到远程分支
$ git push origin <local_branch_name>:<remote_branch_name>
git 
# 删除远程分支
$ git push origin :develop

# 拉取远程分支到本地创建新分支
$ git fetch origin reomte_branch:local_branch_name
$ git checkout -b local_branch_name origin/remote_branch_name
```

## git 管理文件
```
# git 将文件恢复成服务器版本
$ git checkout file_path

# git 放弃本次修改文件
$ git checkout -f 

# git 恢复到某次commit
$ git reset --hard commitid
```


## Git 使用规范流程
```
# 新建分支

# 获取主干最新代码
$ git checkout master
$ git pull

# 新建一个开发分支 myfeature
$ git checkout -b myfeatur

# 提交分支commit

$ git add -all
$ git status
$ git commit --verbose

# 撰写提交信息

Present-tense summary under 50 characters

* More information about commit (under 72 characters).
* More information about commit (under 72 characters).

http://project.management-system.com/ticket/123

# 与主干合并

$ git fetch origin
$ git rebase origin/master

# 合并commit

$ git rebase -i origin/master

$ git reset HEAD~5
$ git add .
$ git commit -am "Here's the bug fix that closes #28"
$ git push --force


$ git commit --fixup  
$ git rebase -i --autosquash 

# 推送到远程分支

$ git push --force origin myfeature

# 发出Pull Request
```


