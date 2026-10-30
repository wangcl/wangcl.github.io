---
title: "免费自定义域名邮箱服务：amelu"
description: 免费账号支持自定义域名邮箱的服务 - amelu.org
date: 2026-10-30T18:01:03+08:00
lastmod: 
categories:
    - Life
tags:
    - email
image: 
math: false
license: 
comments: false
draft: false
build:
    list: always
---

## 免费自定义域名邮箱服务：amelu

amelu 是 ordnary 旗下的邮件服务，free plan 支持的特性如下：

- 1 email domain
- 3 mailboxes per domain
- Unlimited storage per mailbox
- Forwarding & catch-all addresses
- Unlimited address & domain aliases
- Spam filtering & pattern rewrites
- DNS configuration & live health checks
- Community support

这可能是目前对免费用户最友好的邮件服务了，当前已开放注册。

> 服务器位于欧盟境内，国内不能直接访问。

## 使用

amelu 的管理控制台页面是 [app.amelu.org](https://app.amelu.org)，打开后需要用 ordnary 的统一认证登录。

> 首先要使用既有邮箱注册一个 ordnary 账号，登录管理控制台时使用这个邮件地址。

在管理控制台页面可以添加自定义域名，添加时系统询问是否生成默认的 admin, postmaster, abuse 账号（对于标准的邮件服务都是需要的）。如果自动生成，要注意免费账号对每个域名只支持 3 个邮箱，**自动生成的也计算在内**，所以无法继续添加自定义邮箱名了（虽然可以为自动生成的邮箱添加别名）。建议仅保留 admin 账号，postmaster 和 abuse 可以添加为 admin 的别名。这样还可以再添加 2 个自定义邮箱。

配置 DNS 相关信息后，可以使用第三方的邮件客户端添加 amelu 账号:
- imap
  - server: marduk.mx.amelu.org
  - port: 993 (SSL)
- smtp
  - server: marduk.mx.amelu.org
  - port: 465 (SSL)

如果想在浏览器中直接访问，通过 [webmail.amelu.org](https://webmail.amelu.org/)。

> 当前管理控制台还在积极开发中，目前相对简陋，而且服务不稳定。但底层邮件功能已经可以稳定使用了。