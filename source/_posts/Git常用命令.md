---
title: Git常用命令
date: 2018-10-06 11:27:12
tags:
categories: Git
description: 
---

### 仓库
克隆一个仓库

```
git clone <仓库地址>
```

初始化一个仓库

```
git init
```

添加文件到暂存区

```
git add <file_name>  #指定文件
git add .  #工作区的所有文件(过滤文件会过滤掉不需提交的文件)
```

将暂存区文件提交到版本库

```
git commit -m "message"
```

查看工作区的状态

```
git status
```

查看文件被修改的内容

```
git diff
git diff <file_name>  #指定文件
```

查看提交历史	

```
git log
git log --pretty=oneline
```

查看命令历史

```
git reflog
```

关联一个远程库

```
git remote add origin <仓库地址>
```

拉取

```
git pull
```

第一次推送

```
git push -u origin master
```

第一次之后的推送

```
git push origin master
```
查看远程库信息

```
git remote -v
```

### 分支
查看分支

```
git branch
```

创建分支

```
git branch <branchname>
```

切换分支

```
git checkout <branchname>
```

创建并切换分支

```
git checkout -b <branchname>
```

合并某分支到当前分支(合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并)

```
git merge <branchname>
git merge --no-ff <branchname>  #--no-ff参数，表示禁用Fast forward模式
```

删除分支

```
git branch -d <branchname>
```

删除一个没有被合并过的分支(强行删除)

```
git branch -D <branchname>
```

删除远程分支

```
git push origin --delete <BranchName>
```

本地推送分支

```
git push origin <branchname>
```

在本地创建和远程分支对应的分支，本地和远程分支的名称最好一致

```
git checkout -b <branchname> origin/<branchname>
```

建立本地分支和远程分支的关联

```
git branch --set-upstream <branchname> origin/<branchname>
```


查看分支合并图

```
git log --graph
```

### 标签
新建一个标签，默认为HEAD，也可以指定一个commit id

```
git tag <tagname>
```

指定标签信息

```
git tag -a <tagname> -m "message"
```

查看所有标签

```
git tag
```

推送一个本地标签

```
git push origin <tagname>
```

推送全部未推送过的本地标签

```
git push origin --tags
```

删除一个本地标签

```
git tag -d <tagname>
```

删除一个远程标签

```
git push origin :refs/tags/<tagname>
```

### stash命令
假如正在dev分支上的工作进行到一半，还没提交时，需要修复一个紧急Bug，这个时候需要新建临时分支来修复该Bug，但是由于当前的工作内容还没提交，是无法切换分支的，可以先把当前工作现场“储藏”起来

```
git stash
```
查看“储藏”列表

```
git stash list
```
这时候就可以切换分支修复Bug，修复后，再切换到dev分支恢复现场，有两种恢复方法

```
git stash apply  #恢复后，stash内容并不删除，需要用git stash drop来删除
git stash pop  #恢复的同时把stash内容也删除
```

### 撤销修改
- 场景一：当你改乱了工作区某个文件的内容，想直接撤销工作区的修改(让这个文件回到最近一次git commit或git add时的状态)

```
git checkout -- <file_name>
```

- 场景二：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想撤销修改，分两步

```
git reset HEAD <file_name>  #回到了场景一
git checkout -- <file_name>
```

- 场景三：已经提交了不合适的修改到版本库时，想要撤销本次提交(不过前提是没有推送到远程库)，其中HEAD表示当前版本，上一个版本就是HEAD\^，上上一个版本就是HEAD\^\^，当然往上100个版本写100个\^比较容易数不过来，所以写成HEAD~100

```
git reset --hard HEAD^
git reset --hard <commit_id>
```

### 删除文件
1. 同时删除本地文件和远程仓库上的文件

```
git rm <file_name>  #该命令和手动删除效果是一样的
git commit -m 'message'
git push origin master`
```

2. 同时删除本地文件夹和远程仓库上的文件夹

```
git rm -r <folder_name>  #-r 表示递归删除，该命令和手动删除效果是一样的
git commit -m 'message'
git push origin master`
```

3. 删除远程仓库中的文件并保留本地文件

```
git rm --cached <file_name>
git commit -m 'message'
git push origin master
```

4. 删除远程仓库中的文件夹并保留本地文件夹

```
git rm -r --cached <folder_name>  #-r 表示递归删除
git commit -m 'message'
git push origin master
```

> 官方文档：https://git-scm.com/book/zh/v2  
> 参考文档：https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000