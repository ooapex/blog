---
Date: '2021-08-30'
Draft: false
Categories: ['Tech']
Title: WordPress 博客搭建
---

# 分支管理
分支管理的意义是保证有一个分支是一直可以正确使用的，当有新的特性或要修改 Bug 的时候，就创建新的分支，修改好了以后就合并到主分支，保证整个项目的正确运行和访问。

主要分为了两个部分
+ 本地分支管理
+ 远程分支管理

# 本地分支管理-增删改查
本地管理可以新建，修改等操作，但是这些操作都是仅仅在本地，只要不提交到远程仓库，就不会有任何影响。
## 创建新的分支
```
git branch -c newBranch # 仅仅创建新分支
git switch -c newBranch # 创建并切换到新分支
git checkout -b newBranch # 创建并切换到新分支
```
## 切换到新的分支
```
git switch <newBranchName>
```
对分支内容进行修改后，就可以提交到本地分支库。可以使用
```
git add .
git commit -m "modify"
```
## 合并分支
将指定分支的内容合并到当前分支
```
git merge <branchName> 
```

## 删除分支
比如说在 dev 分支修改了新的内容，然后切换会主分支，合并以后，就可以删除 dev 分支了。使用以下命令
```
git branch -d <branchName>
```
# 本地分支管理-冲突解决
如果在 git 进行合并的时候，出现了冲突，那么是因为两个分支都有了新的提交。比如说主分支提交了新的内容，切换到 dev 分支，也提交了新的内容，在切换回主分支进行合并的话，就会出现冲突。

一般都是手动的修改造成冲突的地方，然后再提交，就会有一个新的 commit

# 远程仓库分支管理
## 创建远程分支
```
git push origin oldbranch:<newBranchName>
```
## 建立与远程仓库某分支的链接
```
git checkout -b branch-name origin/branch-name
```
## 将修改提交到分支
```
git push origin <branch-name>
```
## 删除远程分支
```
git push origin :<branchName>
```