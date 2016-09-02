title: Git分支与协作
date: 2014-09-19 16:59:13
categories: Prose
tags: [branch,分支,git]
---
分支功能是git最为强大的功能之一
<!--more-->
查看分支：git branch
创建分支：git branch name
切换分支：git checkout name
创建+切换分支：git checkout -b name
合并某分支到当前分支：git merge name
删除分支：git branch -d name

## 分支管理
合并分支时，git默认使用`Fast forward`模式，这种模式下，删除分支后，会丢弃分支信息。
可以使用`--no-ff`参数禁用，保存分支信息
```shell	
	git merge --no-ff -m "合并信息" dev
```
查看分支历史
```shell
	git log --graph --pretty=oneline --abbrev-commit
```
## 分支策略
master分支应该是非常稳定的，仅用来发布新版本，平时不能再上面干活
dev分支时不稳定的，只在某个版本的时候合并到master上
干活在dev分支上，每个人在自己的分支上干活，时不时合并就可以

## Bug分支
在开发中遇到了Bug，急需修复吗，但Dev分支的任务未完成，不能提交，此时可以保存现场(将改动保存，并清空工作空间)
```shell	
	git stash	# 保存现场
	git status	# 查看状态，工作区已经干净
	git checkout master 	# 切换到主分支
	git checkoutr -b issue-101	# 创建Bug分支
	...			# 修复Bug
	git add .
	git commit -m "fix bug 101"
	git checkout master
	git merge --no-ff -m "merged bug fix 101" issue-101
	git branch -d issue-101
	git checkout dev
	git stash list	# 查看保存内容
	git stash apply stash@{0}	# 恢复到现场
	git stash drop 	# 删除现场缓存
	git stash pop	# 恢复并删除
```
## 多人协作
```shell
	git remote 	# 查看远程库
	git remote -v 	# 显示详细信息
```
### 推送分支
通常只需要推送主分支和dev分支
```shell	
	git push origin master
	git push origin dev
```
### 抓取分支
```shell	
	git clone git@github.com:.... 	# 克隆
	git checkout -b dev origin/dev 	# 创建远程库的dev分支

	git pull 	# 抓取
	git branch --set-upstream dev origin/dev 	# 指定本地分支和远程分支连接
```