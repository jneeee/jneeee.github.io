---
date: 2020-05-30
lastmod: 2022-10-18
title: Git 常用命令
author: Jn
categories: 工作
tags: 
- git
---

> 整理工作场景下常用的git命令

## 一、基本命令


git bash内创建本地 git 仓库等命令
```bash
cd /home/object
git init
# 添加文件
git add readme.md # 跟踪整个文件夹可以使用 git add .
git commit -m'说明' # 提交 add 到的文件，-m 适合简短的说明，一般不加。

git push origin master #origin是默认远程仓库名，master是工作主分支
git status #查看当前文件修改状态
git diff #(可以跟个文件名) 查看文件的 difference
git log # 查看提交历史，如果版本回退过，之前的历史就需要 git reflog
git stash #保存/回退当前已追踪文件的改动，
git stash pop
git stash drop [0]  # 删除stash0
git stash list 
```

## 二、进阶命令

```bash
$ git log #查看最近三次 commit 内容
$ git log readme.md #追踪这个文件的修改记录

$ git reset --hard HEAD^ #回退到上一版本，HEAD 表示当前版本，后面跟上数字表示回退n个版本
$ git reset HEAD file #把暂存区的修改撤销掉

$ git reflog #查看每次的命令（包括回滚操作的）
$ git checkout -- readme.md #readme.md 工作区的修改全部撤销
$ git checkout -- test.txt #错删文件后恢复
$ git checkout <commit_id> <file_name>

$ git rebase -i <commit_id> #变基
$ git commit --amend [--no-edit] # 把最近的修改合并到上次的commit里，可选[不改说明]
```

## 三、远程仓库的本地操作

参考https://code.aliyun.com/help/ssh/README
```bash
$ cat ~/.ssh/id_rsa.pub #判断本地是否已有 sshkey
$ ssh-keygen -t rsa -C "youremail@example.com" #创建一个key，接下来可以一路enter 不用密码。
$ cat ~/.ssh/id_rsa.pub #获取key，也可以到目录下打开复制。
$ git remote add origin <git@项目地址> #添加远程仓库
$ git push -u origin master
$ git remote set-url origin <new-link>
```
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

## 四、分支管理

Git鼓励大量使用分支：
```bash
$ git checkout -b dev # 创建并切换到，-b相当于branch。
$ git branch dev #创建分支，不跟参数为查看当前分支信息
$ git merge [--squarsh] dev #合并指定分支到当前分支
$ git branch -d <name> #删除分支
```
遇到分支合并时文件冲突需要手动解决！如果在使用 `git merge dev`时候都会提示冲突。需要手动修改冲突文件。

一般开发场景不鼓励直接在主分支修改，而是创建一个<bugid>分支，改好了切到主分支使用 git merge 指令。



