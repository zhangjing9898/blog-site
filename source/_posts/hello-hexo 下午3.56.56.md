---
title: git 常见命令
date: 2018-07-10 15:44:30
tags:
- git
---

# git常见命令

### 先讲讲如何clone一个远程repertory。  <br />
```
$ git clone xxx(repertory ssh地址)
```

### 接下来讲讲分支相关的命令。  <br />

- 新建一个分支，并切换到该分支：
```
$ git checkout -b xx(branch name)
```
- 查看分支：
```
$ git branch
```
- 查看远程所有分支：
```
$ git branch -a
```
- 删除分支：
```
$ git branch -d xxx(branch name)
```
### 如何进行回退

- 回退版本：
```
$ git reset --hard version(commit number)
```

### 查看状态

- 查看状态
```
$ git status
```

### 添加更新，推上去

- 保存
```
$ git add .   (这是将所有修改过的工作文件提交暂存区)
```
```
$ git add     (将工作文件修改提交到本地暂存区)
```

- commit
```
$ git commit -m 'xxx'(里面是修改的信息的info)
```
- push
```
$ git push xxx(该仓库的ssh地址)
```

### 合并某分支到当前分支

```
$ git merge xx(name)
```
### 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改
```
$ git reset --hard
```

### 打开ssh文件

```cli
$ cat ~/.ssh/id_rsa.pub 
```