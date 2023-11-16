---
title: VaultWarden部署
date: 2023-11-16 21:50:44 +0800
categories: ['Personal', 'Warden']
tags: ['密码管理器']
---

>本来想部署BitWarden的,部署过程中发现安装sqlserver时至少需要2000MB的内存,于是就安装不下去了,后看到v2ex有篇帖子推荐使用bitwarden_rs,于是就在服务器上搭建了bitwarden_rs

## 安装

```bash
docker run -d \
--name vaultwarden \
-v /vw-data/:/data/ \
--restart unless-stopped \
-p 10080:80 \
vaultwarden/server:latest
```

## 设置反代

可以通过nginx或者caddy进行反代并添加https证书

1. 通过域名进行访问网页版
2. 创建用户
3. 绑定邮箱
4. 开启二次验证
## 使用

1. 下载bitwarden浏览器插件
2. 选择<kbd>自托管</kbd> ,输入反代后的域名
3. 登录之后就可以愉快的使用了

## 现状

现在密码管理的账户比较少,等后面变多了需要做一个定时导出密码,要不然哪天服务器崩了就完蛋了