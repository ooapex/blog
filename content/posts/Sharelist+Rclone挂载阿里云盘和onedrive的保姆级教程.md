---
Date: '2021-08-09'
Draft: false
Categories: ['Tech']
Title: WordPress 博客搭建
---

> 基于 sharelist，挂载阿里云盘，onedrive，google drive 等，搭建私人不限速网盘。**文章末尾有 microsoft 5 T 子号相送**



[toc]

# 前言

自己平常看到有优惠活动，就申请了许多的网盘，有 onedrive，google drive，aliyunpan，teambition，蓝奏云等。就挺多的，但是也没有好好的利用，单单是阿里云就能有 10 T 的空间。因为存在以下问题：

+ 每次要下载个文件什么的，都需要登录客户端，然后下载，也用不了我的 aria 2 或者 IDM，虽然速度确实是跑满的，但是这么多网盘，使用体验确实没有那么好。
+ 而且，有时候想发一些文件给别人，一时间也找不到好的方式，只能通过 QQ 微信发送，很麻烦。
+ 自己网盘的空间得不到充分的利用，闲置很多

于是乎，就有了拿这些网盘，挂载到一起，做一个大的网盘的想法。搜了网上的一些资料，有个很好用的软件 [Rclone](https://rclone.org/), 还有 [sharelist](https://github.com/reruin/sharelist), 前者可以将 onedrive、google drive、dropbox 等的挂载为本地磁盘，后者拥有设计精美的前端页面和后台管理系统，可拓展性强，搜寻资料，历经千难万险，终于摸索出了挂载这些网盘到前端页面的教程，使用方便，上传下载都很方便。

本套教程包括了：

+ sharelist 挂载阿里云盘
+ rclone 挂载 onedrive 到 linux 硬盘
+ sharelist 挂载本地硬盘
+ 持续更新

最终效果：

网址: https://pan.i52.me

![](https://gitee.com/agcl/oss/raw/master/img/20210809162412.png)

# 宝塔面板安装

> 其实，只要你足够熟悉 nginx 的相关配置命令，是不需要安装宝塔面板的。

```shell
Centos安装脚本:yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
Ubuntu/Deepin安装脚本: wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh
Debian安装脚本: wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh
Fedora安装脚本: wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh
```

然后就等着就行了，等到脚本执行完出现了访问链接和密码，点击链接进入浏览器，就能够看到宝塔的登录面板了，默认安装 lnmp 运行环境，主要要用的就是个 nginx，如果不安装这些环境也无伤大雅。

![](https://gitee.com/agcl/oss/raw/master/img/20210809164103.png)

![](https://gitee.com/agcl/oss/raw/master/img/20210809164018.png)



# Sharelist 下载与安装

[sharelist](https://github.com/reruin/sharelist) 是由 reruin 开发的一款快捷挂载网盘的工具，支持挂载 google drive，onedrive 等。支持上传，下载，预览。**注意，所有的网盘挂载后都支持下载预览等，但是只有以 `OD API` 方式挂载的 onedrive，`google API` 方式挂载的 Google drive，本地磁盘挂载这三种方式挂载的磁盘支持上传。**

去 Baidu 或者 Google 搜索，所有的教程都是千篇一律的。你复制我，我复制你，什么有用的信息都没有，官方的文档也不够详细，解决不了我的问题。于是乎我就只能自己探索了，以下是在安装和配置过程中遇到的问题记忆解决方法。

## 安装-官方方法 [Doc](https://reruin.github.io/sharelist/docs/#/zh-cn/)

> 需要：一个 VPS，一个宝塔面板，一个域名，一双灵巧的手

官方方法大同小异，下面的教程大家可以照做，我直接复制下来以免在博客之间跳来跳去。

```shell
#Debian/Ubuntu系统
apt-get -y install git

#CentOS/RHEL系统
yum -y install git

#下载源码(国内国外下载源任选一个)
git clone https://gitee.com/kenvie/sharelist.git   #国内
git clone https://github.com/reruin/sharelist.git  #国外
#执行安装
cd sharelist && bash install.sh
```

### 坑 1：sharelist 33001 无法访问？

执行完上面的命令，sharelist 就已经启动了，官方的教程会告诉你，访问 ip: 33001 就能够看到你的 sharelist，实际上，安装了宝塔面板后，这么做是不行的。需要做两步：

+ 如果你是腾讯云或阿里云等国内云服务器，首先需要去打开 `33001` 这个端口。
+ 接着，在宝塔的安全选项下，也要打开 33001 这个端口才行。

![](https://gitee.com/agcl/oss/raw/master/img/20210809164509.png)

![](https://gitee.com/agcl/oss/raw/master/img/20210809164622.png)

这样，才能使用 ip: 33001 的格式来启动 sharelist。

但是，拿 IP 访问，还带个端口，终究是上不了台面不太好看的，于是乎官方建议使用 nginx 反代，让链接更加的好看一点, 操作很简单.

1. 在宝塔中新建一个站点，啥也不需要，纯静态的站点，不需要 PHP 等环境，不需要数据库。

![](https://gitee.com/agcl/oss/raw/master/img/20210809165314.png)

2. 点击设置，设置反向代理。

![](https://gitee.com/agcl/oss/raw/master/img/20210809165514.png)

3. <img src="https://gitee.com/agcl/oss/raw/master/img/20210809165638.png" style="zoom:80%;" />

4. 添加完毕以后，点击配置文件，在域名的后面添加以下的配置。

```
	# 反代添加内容start
    proxy_set_header Host  $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    client_max_body_size 8000m;
  
    proxy_set_header Range $http_range;
    proxy_set_header If-Range $http_if_range;
    proxy_no_cache $http_range $http_if_range;
    # 反代添加内容end
```



![](https://gitee.com/agcl/oss/raw/master/img/20210809170307.png)

好了，去解析你的域名到你的 vps 的 IP，就可以拿域名访问了。

访问进去的基本设置就不多说，无非就是设置密码口令（自己记住），网盘名称等，首先网站会给一个样例，挂载本地硬盘。直接点击保存就能够进入 sharelist 前端的页面了。

![](https://gitee.com/agcl/oss/raw/master/img/20210809175804.png)

## 非官方安装方法-一键脚本（推荐）

```
bash -c "$(curl -sS https://www.cooluc.com/sharelist-install.sh)"
```

由某位大佬开发，内置了许多的主题，教程地址https://media.cooluc.com/source/sharelist

# Sharelist 挂载阿里云盘

## 添加挂载源

在 sharelist 后台选择 `阿里云盘(beta)`, 名称随便填，然后点击保存，然后进入主页。

![](https://gitee.com/agcl/oss/raw/master/img/20210809180736.png)

主页会出现刚才挂载的阿里云网盘，点击目录完成一些基本配置。

![](https://gitee.com/agcl/oss/raw/master/img/20210809180805.png)

## 获取阿里云盘 token

然后要**获取阿里云的 token：**

1、进入[阿里云盘手机页面](https://passport.aliyundrive.com/mini_login.htm?lang=zh_cn&appName=aliyun_drive&appEntrance=web&styleType=auto&bizParams=) 打开 F 12 开发工具，登录阿里账号密码，点击登录的时候，开发工具网络中会新加载一个 login. Do 文件，右键复制 -> 复制响应

2、进入 [token解码](https://media.cooluc.com/decode_token/) 粘贴响应数据，点击解码，复制弹出的 token，粘贴到 sharelist 的 token处

感谢@ [博主的](https://www.cooluc.com/) 的解码工具![](https://gitee.com/agcl/oss/raw/master/img/20210809181106.png)



根据上面的教程获取的 token 就是正确的 token，要不然可能会报下面的错误。

### 坑 2：文件无法下载，会报 xml 没有请求头的错误

除了上面的 token 设置不正确之外，一般情况下，自己的 vps 服务器是没有 ssl 的，而 cloudflare 是可以提供 ssl，确实是很方便。但是对于 sharelist，如果只是客户端到 cloudflare 服务器收到 ssl 加密，而 cloudflare 到 vps 服务器的连接没有 ssl 加密，那么当从 sharelist 上下载文件时，会出现链接不匹配，参数为空等错误。最典型的是下面的

```xml
This XML file does not appear to have any style information associated with it. The document tree is shown below.
```

反正就是下载和预览都是不行的，都会出现类似的错误，到这里小心是不是 SSL 的问题了。

# Sharelist 挂载 onedrive

官网提供了各种各样的方法来挂载 onedrive，但是我试了一下以下几种。

+ ~~OD API：不管怎么样都挂载不成功，每次都告诉我 reply API 设置不正确，我放弃了。~~（我又可以了 2021-08-09）
+ Onedrive v 2：这个很容易就能挂载成功，但是缺点就是不能上传，我也放弃了。
+ **后来使用 rclone 将 onedrive 挂载为本地硬盘。使用 sharelist 挂载本地硬盘，实现了最终效果，可上传下载预览。**

##  使用 OD API 挂载

OD API 方式挂载的 onedrive，可以上传下载预览，而且不会走服务器的流量，可以说是理想之选。但是在实际的挂载过程中出现过很多的问题，其中最常见的就是`reply url`错误。找了很多的地方都没有找到解决的方法。

最后发现，是因为 OD API 方式挂载的插件的原因，一致导致挂载失败。

**注意：使用 OD API 挂载时，挂载的名字不能是`od` 或者`onedrive`或者`odrive`等，因为这是插件里面设置的关键字，会导致挂载出错**

**注意：使用 OD API 挂载时，挂载的名字不能是`od` 或者`onedrive`或者`odrive`等，因为这是插件里面设置的关键字，会导致挂载出错**

**注意：使用 OD API 挂载时，挂载的名字不能是`od` 或者`onedrive`或者`odrive`等，因为这是插件里面设置的关键字，会导致挂载出错**

然后就简单了，只需要几个步骤。

### 访问后台注册应用

指定路径，这里指定的是 a，那么在 microsoft 的后台，到时候指定的也只能是 a, 设置好后台之后，点击保存，回到前端，点击 a 目录。

![](https://gitee.com/agcl/oss/raw/master/img/20210809210634.png)

点击 azure 管理后台，然后注册应用。

![](https://gitee.com/agcl/oss/raw/master/img/20210809210437.png)

注册应用的配置如下：

![](https://gitee.com/agcl/oss/raw/master/img/20210809210902.png)

注册完后会跳转到下一个界面，然后就需要获取 app id 和 app_secret. 前者的获取很容易

![](https://gitee.com/agcl/oss/raw/master/img/20210809211026.png)

后者的获取需要进入`证书和密码，生成密钥才能行。`

![](https://gitee.com/agcl/oss/raw/master/img/20210809211205.png)

复制下面的密钥，结合上面的 app_id，填入上面的 sharelist 认证页面中，一直点击下一步，就成啦。

![](https://gitee.com/agcl/oss/raw/master/img/20210809211243.png)

刷新自己的云盘，再次点击进入 a 目录，就能够看到自己已经挂载好的 onedrive 了。

## Sharelist+rclone+onedrive+vps 挂载

采用 rclone 将 onedrive 挂载为本地磁盘，然后再使用 sharelist 的本地磁盘挂载功能将挂载为本地盘的 onedrive 挂载到前端页面上，这样实现简单，但是会走服务器流量，服务器流量少的慎用。

### Linux 使用 Rclone 挂载 onedrive 成为本地硬盘

参考[博客](https://www.misterma.com/archives/900/)，写的很好很完善，一步到位

### Sharelist 挂载本地硬盘

上一步将 onedrive 挂载为本地硬盘之后，可以使用`df -h`查看自己服务器的容量，很容易就能看到自己新挂载的网盘有多大。

然后进入 sharelist 后台页面，添加新的挂载源，操作跟挂载阿里云类似。

![](https://gitee.com/agcl/oss/raw/master/img/20210809182609.png)

然后就可以看到你的 onedrive 了，可以点击右上角的目录上传和下载文件，十分的简单，而且，不走服务器的流量，所有就算是 vps 也不用怕。

# 总结

本文介绍了使用 sharelist 挂载阿里云盘和 onedrive 的方法，聚合网盘还是挺好用的，下面是我搜集的一些大佬搭建的网盘地址，有一些资源大家可以自行下载。

+ https://ali.kenvie.com

+ https://pan.jindoucloud.top

+ https://drive.kenvie.com

+ https://media.cooluc.com

> 前人种树后人乘凉，感谢各位大佬的奉献

# 福利

为了测试挂载 onedrive 的问题，所以就申请了一些巨硬（微软）账号，每个账号都有 5 T 的 onedrive 空间，和 office 365 全家桶。需要自己搭建的网盘的或者需要 office 应用的，可以在下面留言邮箱以及想要的前缀，定期会给大家开号并发送到邮箱。

申请格式：

```
前缀: ishland
邮箱： email@example.com  # 用于接收账号密码
```