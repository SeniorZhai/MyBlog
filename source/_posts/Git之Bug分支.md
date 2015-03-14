title: Git之Bug分支
date: 2015-03-09 21:15:52
categories: Prose
tags: [Git,分支]
---
在开发中常常遇到这种情况——你正在指尖飞舞，码代码ing，这个时候QA来了个BUG。这个时候，工作区里还有很多ing的代码，没有提交，你又需要创建分支来解决BUG。
<!--more-->
这个时候需要`git stash`命令来拯救你的代码
这条命令可以把当前工作现场`储存`起来，等以后恢复现场后继续工作
储存后，打出分支解决问题，完成后，合并分支(git merge)，再删除分支(git branch -d)
之后用`git stash list`查看储存起来的工作现场
这个时候可以使用下面两个方法恢复工作现场：
1. `git stash apply`恢复，`git stach drop`删除工作现场
2. `git stash pop`恢复