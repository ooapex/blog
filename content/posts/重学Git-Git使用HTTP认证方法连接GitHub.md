---
Date: 2021-08-24
Draft: false
Categories:
  - Tech
Title: 重学Git-Git使用HTTP认证方法连接GitHub
---

# Git 配置

当下载完 Git，安装好以后，需要对 git 进行一个基本的配置。才能完成跟 GitHub 的交互。
## 新建一个仓库
去 GitHub 新建一个仓库，如 [HelloWorld](https://github.com/Jeffrey-D/HelloWorld.git)
然后复制仓库的链接，到安装好 Git 客户端的命令行执行 `clone` 命令。
```
git clone https://github.com/Jeffrey-D/HelloWorld.git
```
## Config 基本配置
进入此项目，执行以下命令，并做一些基本的配置

```shell
cd HelloWorld  # 进入项目目录

# 个人信息配置
git config --global user.name "ishland"
git config --global user.email "example@example.com"
```
上面配置了提交的用户名和邮箱。**注意，个人邮箱要与 GitHub 个人信息界面的邮箱列表中的邮箱一致**，然后对项目做一些修改，比如修改 [README](../README.md) 等等。输入以下命令提交修改。
```shell
git add . # 将修改做一次stage，也就是记录

# 对上面的add做一次提交，也就是一次快照，使用-m 指定本次的提交说明信息
# 此时只是给你的修改"拍了照片"，只是记录了下来，并没有提交你的GitHub仓库上。
git commit -m "update readme" 

# 提交到远程仓库，直接使用git push
git push
```
上面可以直接使用 git push 的原因是这是 clone 的项目，在项目的根目录下已经存放好了一个隐藏文件，内包含了项目的信息，所以使用 git push 会直接提交 clone 时的默认的分支，也就是 main。

首次使用 git push 时，会让你输入 PAC (`Personal Access Token`) 密钥, 在个人设置 setting->developer settings->personal access token 下可以创建生成，每个 PAC 都只会在创建的时候可见，所以第一次创建的时候最好记住。

# 本地缓存 PAC
## 官方推荐方法
在最新版的 Git 中，已经包含了 Git Credential Manager Core (GCM Core) 来存储和进行身份验证，防止重复输入 PAC。首次使用 Git 进行 push 操作时，会提示你输入你的 PAC，或者弹出浏览器让你认证某个 APP，一旦认证完成，你的 PAC 密钥将由 GCM Core 来保存管理，只要不再修改 PAC，那么以后的所有的 git 操作都不需要再输入 PAC 了。
## 个人命令缓存
本方法不在官方的推荐方法中，但是依旧可以使用，不过需要手动输入命令。使用以下命令全局让本地缓存 PAC，
```
git config --global credential.helper store  # 永久磁盘明文缓存
git config --global credential.helper cache  # 15分钟内存缓存
```
以后进行提交操作的时候，就不会再次要求输入 PAC 了，可以在同一个项目的目录下进行基本的操作。

当你不需要本地存储 PAC 时，可以使用以下命令取消本地的存储
```
git config --global 
```



