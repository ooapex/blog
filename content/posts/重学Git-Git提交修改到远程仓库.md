---
Date: '2021-08-30'
Draft: false
Categories: ['Tech']
Title: WordPress 博客搭建
---

# 提交修改到仓库
使用 `git push` 命令, 将已经 `commit` 的修改提交到远程仓库，本命令有两个参数：
+ 远程链接名, 也就是通过 `remote add` 添加的远程仓库名字如 `origin`
+ 仓库的分支名，如 `main`

连在一起就是:
```
git push origin main
```

# 重命名远程分支
分为三步，第一步是先删除远程的分支，第二步是重命名本地的分支名字，第三步将重命名的分支提交到远程仓库，
+ 删除远程分支
    ```
    git branch -r # 产看远程分支名
    
    # 删除远程分支
    git push origin :remoteBranchName  #删除远程分支,方法1
    git push --delete origin :remoteBranchName #方法2
    ```
+ 重命名本地分支
    ```
    git branch -l # 查看本地分支名

    # 将本地的branchName重命名为新的newBranchName
    git branch -m branchName newBranchName
    ```
+ 提交命名的修改
    ```
    git push origin newBranchName
    ```
通过以上的命令就可以创建新的分支。那么将修改提交到新的分支就只要用 `push` 命令就行了。
```
git push origin newBranchName
```
这样就只会提交到新的指定的分支，而不会提交到旧的分支

# 提交 Tag
提交 tag 前要先创建 tag，可以使用 `git tag` 系列命令创建，后面会详细介绍 tag 管理，此处只是简单的提交
```
git push <REMOTENAME> <TAGNAME>
```
或者提交所有的 tag
```
git push <REMOTENAME> --tags
```

# 合作项目获取
有的项目想要是合作，那么可以自己设置合作项目的远程链接，再获取项目的内容。
```
# 设置远程链接，一般使用upstream这个名字
git remote add upstream  <THEIR_REMOTE_URL> 

# 然后使用git fetch
git fetch upstream
# Grab the upstream remote's branches
> remote: Counting objects: 75, done.
> remote: Compressing objects: 100% (53/53), done.
> remote: Total 62 (delta 27), reused 44 (delta 9)
> Unpacking objects: 100% (62/62), done.
> From https://github.com/octocat/repo
>  * [new branch]      main     -> upstream/main
```
> 注意，使用 `git fetch` 只会下载项目，而不会跟本地已有的项目合并
