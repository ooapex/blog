---
Date: 2021-08-03
Draft: false
Categories:
  - Tech
Title: Markdown写作个人图床配置-Gitee，SM.MS
---

# 前言
写作最重要的是资源，不像是古老的在本子上写作，或者是在本地写作，现如今的许多的服务都是托管到其他的服务平台上。就像是我的博客，是搭建在别人的虚拟主机上一样。万一哪一天我的主机服务商跑路了，那我的所有的资源将付之一炬，什么也不剩下了。
因此，是很有必要将自己的文字和插入的图片进行备份的，~~虽然目前啥也没~~，但是，提早的做好预防总是不错的。
Wordpress 有自带的插件，可以定期备份（All in One WP Migration），我试了使用这个插件整体迁移，效果还是很不错的，几乎所有的配置（主题，文章，设置等），都可以顺利的迁移过来，使用体验很好。
但是，我目前使用的是虚拟主机，wordpress 是个一体化的写作软件，也就意味着所有的文章图片都是存储在我的这个虚拟主机上。虚拟主机提供的存储空间是有限的，随着时间的推移，我上传的图片啊或者媒体资源的变多，就会出现空间危机。~~为了优雅的白嫖，那就开始自建图床吧~~
# 准备工作
虽然说是自建图床，其实就是利用大厂的资源罢了。看了许多的厂子的对象存储的价格，都很便宜，流量也不贵，但是心里总是有点虚。
1. 阿里云：最低 9 rmb/y, 40 G 的存储空间，下行流量 0.25 r/G
2. 腾讯云：注册免费送 60 G 存储空间，持续半年，半年后再续费
3. 又拍云：需要自己有备案的域名，望而却步，**当然如果有了自己备案的域名，可以加入又拍云联盟，每个月有免费额度，十分够用**
4. 七牛云：业界评价很好，还是需要有备案域名，望而却步
5. SM. MS：免费，有免费的 API，可以用一用
6. GitHub：免费，做国外的博客应该可以吧，国内的就算了，访问堪忧
7. Gitee：免费，国内访问速度很不戳，可以用一用
综上所述，为了优雅的白嫖还保证自己的图片的安全，选择了 Gitee 作为主要的存储池，选择 SM. MS 作为备用的。

# Gitee 账号准备
## 新建仓库
首先注册 Gitee 账号，国内平台，使用手机号注册即可，然后就点击右上角新建仓库，仓库配置如下，**注意要选择下面的新建 master 分支，这个比较主要**：
![](https://gitee.com/agcl/oss/raw/master/img/20210803170048.png)
## 生成私人令牌
私人令牌就是一段加密的代码，可以理解为一段私钥，可以让你不用登录 Gitee 就能够在命令行或者其他工具里面操作你的项目，十分的方便。
生成私人令牌的步骤如下：
1. 点击设置，找到私人令牌选项，点亿下
	![](https://gitee.com/agcl/oss/raw/master/img/20210803170502.png)
2. 点击生成新的令牌
![](https://gitee.com/agcl/oss/raw/master/img/20210803170615.png)
3. 创建令牌，令牌描述随便写，下面的选项一般只选一个 project，看个人情况选择
![](https://gitee.com/agcl/oss/raw/master/img/20210803170810.png)
4. **复制生成的令牌或者保留页面**，一定要现场复制，因为这个 token 只会出现一次，所以当你离开页面以后，再想找就找不到了。只能重新再生成了。
![](https://gitee.com/agcl/oss/raw/master/img/20210803171120.png)
# PicGo 准备
## 软件下载
PicGo 是一个支持快捷上传图片到上述一些图床的软件，可以直接在 Github 上找到，去 release 页面下载相应的平台的对应的软件。
[release 页面](http://https://github.com/Molunerfinn/PicGo/releases "release")
我是 windows，所以我选择的是以下版本
![release版本](https://gitee.com/agcl/oss/raw/master/img/20210803171501.png)
## 配置
### Gitee 配置
打开 picGo，点击插件设置，安装 gitee 插件
![](https://gitee.com/agcl/oss/raw/master/img/20210803171759.png)
接着点开图床设置，翻到最下面找到 Gitee，填写刚才创建的 gitee 仓库和私人令牌的配置。
![](https://gitee.com/agcl/oss/raw/master/img/20210803172130.png)
+ repo：用户名带项目名
+ branch：master
+ token：私人令牌
+ path：随便写
+ costomPath 和 coustomURL：随便写

然后设为默认图床，点击确定，完工。

### SM. MS 配置
进入以下链接，生成 SM. MS 的 token（当然你需要先注册） https://sm.ms/home/apitoken
然后点击 picGo 里面的 SM. MS 图床，粘贴 Token 就行了，很简单。
![](https://gitee.com/agcl/oss/raw/master/img/20210803172455.png)

# 快捷键
Windows 下，只要 picGO 在后台运行着，自己已经截图好了（推荐使用 snipaste, 简直是 windows 截图神器）。只需要按'''Ctrl + Shift + P'''即可快捷上传，然后啥都不用管，图片链接就在你的剪贴板了，直接回到文档里面粘贴就行了，不可谓不爽。
使用体验如下：
截图->快捷键上传->回到文档粘贴（我用的 markdown 格式）->图片实时展示出来
完美
Enjoy It~







