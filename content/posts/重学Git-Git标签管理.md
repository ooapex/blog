---
Date: '2021-08-30'
Draft: false
Categories: ['Tech']
Title: WordPress 博客搭建
---

每个标签都是跟随着每个 commit 的，commit 后可以给本次提交打上标签。
# 本地操作
## 创建标签
```
git tag <tagname>
```
## 删除标签
```git
git tag -d <tagname>
```
## 查看标签
```
git show <tagname>
```
## 给指定 commit 打标签
```
git tag <tagname> <commitID>
```
# 远程操作
## 提交到远程
```
git push origin <tagname>
git push origin --tags # 提交所有标签
```
## 远程删除
先本地删除，再远程删除
```
git push origin :refs/tags/<tagname>
```
