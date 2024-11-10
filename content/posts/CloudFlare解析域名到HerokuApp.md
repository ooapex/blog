---
Date: 2021-08-05
Draft: false
Categories:
  - Tech
Title: CloudFlare解析域名到HerokuApp
---

# 前言

## [CloudFlare](https://dash.cloudflare.com/) -Web Performance & Security

不得不说，Cloudflare (以下简称 CF) 解析域名是真的方便，我把所有的域名都托管到 CF，然后可以在 CF 上统一管理，统一解析，用起来还是很舒服的。

![](https://gitee.com/agcl/oss/raw/master/img/20210805100337.png)

CF 似乎还提供免费的 SSL（是啥我也不知道，后面会了解一下，写个博客），那样我们的网站就有了一个小🔒，能够防止攻击什么的。CloudFlare 能够完全托管我们的域名，免费的计划我觉得已经够我使用了，能够在看到域名的访问信息，缓存信息等，吹爆。

![](https://gitee.com/agcl/oss/raw/master/img/20210805100643.png)

## [Heroku](https://dashboard.heroku.com/) -云应用平台

Heroku 是我前两年注册的一个平台，当时还验证了卡，想着免费也没什么用，就一直放着没动，前两天折腾 VPS 的时候，逛 GitHub 发现了有一些应用可以一键部署到 Heroku 上，想着这么方便，我就试一试呗，然后就有了以下的两个网站

[小木屋书笺](https://blog.i52.me/)

[发卡网](https://faka.i52.me/#/)

前一个是基于 Ghost 搭建的博客网站，后面一个是一个博客网站，以后有什么可以卖的时候再完善一下发卡网站叭。

总的来说，一个 heroku 账号，如果不绑卡验证，所有应用加起来每个月可以有 550 小时的运行时间，大约全天 24 小时不间断可以运行 21 天，如果验证了卡，每个月会再加上 450 小时的运行时间，也就是总共有 1000 小时的运行时间，折合 41 天。然后 heroku 还有一个很特别的机制，就是为了防止滥用，应用长时间没有访问的话，会进入休眠，不计算相应的运行时间，时间上还是够够的。

博客应用还是常驻的好，后面想办法能不能保证应用不自动休眠且自己也尝试写个 heroku 部署脚本出来😄

# 域名解析

Heroku 应用自带域名解析配置，提供两种方法将托管于 CF 的域名解析到 Heroku。

## 直接解析到 Heroku 分配的域名上

一般新建好 App 以后，heroku 会自动给你分配一个域名，类似于 `appname.herokuapp.com`, 其中的 `appname` 就是你创建 app 时指定的名字，第一种解析方法就是再 cf 上的 dns 解析下，添加记录

![](https://gitee.com/agcl/oss/raw/master/img/20210805100800.png)

添加以下记录：

![](https://gitee.com/agcl/oss/raw/master/img/20210805100817.png)

## 解析域名到 heroku 的 dns 服务器

在 heroku 的控制台下，点击进入你要配置域名的 app，再再 setting 选项卡的最下面，会有一个域名配置，可以用来直接指定域名，然后再将 cf 的解析指向 heroku 提供的域名解析服务。

1. 先找到 setting

![](https://gitee.com/agcl/oss/raw/master/img/20210805100838.png)
2. 滑动到最下面会有 domain 配置选项卡

    ![](https://gitee.com/agcl/oss/raw/master/img/20210805100914.png)

3. 添加自己要解析的域名

    ![](https://gitee.com/agcl/oss/raw/master/img/20210805100932.png)

4. 配置好点击 next 就会生成 dns target，复制下来

    ![](https://gitee.com/agcl/oss/raw/master/img/20210805100945.png)
5. 回到 CF，按照方式 1 一样的方法，设置为 CNAME，注意，自己域名的解析记录要跟 heroku 上配置的一致，比如说我在 heroku 上配置的是 blog. I 52. Me，那么在 cf 上，记录就应该天 blog，目标就填刚刚复制的 dns target。点击保存完工，稍等一会儿就可以看到自己的域名解析到对应的 app 了。

    ![](https://gitee.com/agcl/oss/raw/master/img/20210805100959.png)

# 总结

CF 很香，Heroku 也很不错，以后要折腾的地方很多

- [ ]  了解 CF 中的 SSL/TLS 是什么和相关机制
- [ ]  配置 Heroku 的 app 不休眠