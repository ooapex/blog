---
Date: '2021-08-24'
Draft: false
Categories: ['Tech']
Title: WordPress 博客搭建
---

# 远程仓库
远程仓库就是存储项目代码文件的地方，由于认证方式的不同，远程仓库的链接可以分为两种：
+ HTTPS 链接： https://github.com/user/repo.git
+ SSH 链接： git@github.com : user/repo. Git

# 给远程仓库（链接）起名字
再 Git 的实际使用过程中，如果每次提交给远程仓库都使用链接的话，那会显得很长，且当有多个仓库在管理时，就会显得很麻烦。所以 Git 提供了一个给远程仓库重新起个简单的名字的操作，也叫做**创建远程仓库**，其实就是将本地的某个名字与远程的某个仓库对应起来。
```
git remote add origin <REMOTE_URL>
```
其中 `git remote add` 是标准命令，后面的 `origin` 指的是远程仓库的名字，随便指定，`<REMOTE_URL>` 指的是远程的链接。

## 远程仓库名字修改
正常情况下，`git push` 会有两个参数，分别是远程连接名和分支名。如 `git push origin main`, 意思就是将修改提交到远程连接名为 origin 的 main 分支。

Origin 是给某个远程仓库的链接指定一个名字，可以使用命令行修改远程连接的名字
```
$ git remote -v
# 查看现有远程
> origin  https://github.com/OWNER/REPOSITORY.git (fetch)
> origin  https://github.com/OWNER/REPOSITORY.git (push)

$ git remote rename origin destination
# 将远程名称从 'origin' 更改为 'destination'

$ git remote -v
# 验证远程的新名称
> destination  https://github.com/OWNER/REPOSITORY.git (fetch)
> destination  https://github.com/OWNER/REPOSITORY.git (push)
```

## 名字所对应的链接修改
Origin 所指定的远程链接也可以修改
```
$ git remote -v
> origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
> origin  git@github.com:USERNAME/REPOSITORY.git (push)
$ git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
$ git remote -v
# Verify new remote URL
> origin  https://github.com/USERNAME/REPOSITORY.git (fetch)
> origin  https://github.com/USERNAME/REPOSITORY.git (push)
```
同样可以使用以上命令将 HTTP 远程连接切换为 SSH 远程链接。

# 删除远程链接
当不需要使用当前的远程连接名字时，可以使用 `git remote rm <Origin Name>` 删除。
可见以下示例
```
$ git remote -v
# View current remotes
> origin  https://github.com/OWNER/REPOSITORY.git (fetch)
> origin  https://github.com/OWNER/REPOSITORY.git (push)
> destination  https://github.com/FORKER/REPOSITORY.git (fetch)
> destination  https://github.com/FORKER/REPOSITORY.git (push)

$ git remote rm destination
# Remove remote
$ git remote -v
# Verify it's gone
> origin  https://github.com/OWNER/REPOSITORY.git (fetch)
> origin  https://github.com/OWNER/REPOSITORY.git (push)
```
注意，这只是删除了本地的一个链接映射，远程的服务器上的仓库不会受到影响。