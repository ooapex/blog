---
Date: '2021-09-10'
Draft: false
Categories: ['Tech']
Title: WordPress 博客搭建
---

## 错误信息

WARNING: Retrying (Retry (total=0, connect=None, read=None, redirect=None, status=None)) after connection broken by 'SSLError ("Can't connect to HTTPS URL because the SSL module is not available.")'

## 错误描述

**windows 环境下**安装了**minianaconda**，然后使用 pip 安装包出现 ssl 错误，原因不明，**有可能是环境变量丢失造成的**

## 分析原因

Dll 文件缺失惹的祸

## 解决办法

```shell
将  `/minianaconda/Library/bin` 目录下的
    libcrypto-1_1-x64.dll
    libcrypto-1_1-x64.pdb
    libssl-1_1-x64.dll
    libssl-1_1-x64.pdb
四个文件复制到 `minianaconda/DLLs`目录下
```

完工
