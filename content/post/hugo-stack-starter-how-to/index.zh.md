---
title: "How-To: Hugo Stack Starter"
description: 使用 Hugo Stack Starter Template 在 Github Pages 上发布个人主页
date: 2026-09-11T09:49:17+08:00
lastmod: 2026-09-13
math: false
categories:
    - Tech
tags:
    - Hugo
    - Stack
draft: false
build:
    list: always
---



## 操作步骤

1. 打开 [hugo-stack-starter](https://github.com/liu-houliang/hugo-stack-starter) 仓库（目前是 1.0.1 Release 版本），点击右上角绿色的 "Use this template"

2. 创建新仓库时，命名为 "用户名.github.io"（如有旧版本，建议将旧版本仓库改名备份）

3. 进入新仓库，点击 Settings -> Pages，将 Source 改为 "GitHub Actions"

4. 修改网站框架内容

   需修改的文件主要位于 `config/_default/` 目录下：

   - `config.toml`
     - `baseurl`：必须改成自己的主页地址 "https://用户名.github.io/"
     - `title`
     
   - `languages.toml`
     - `title`
  
   - `menu.zh.toml` | `menu.en.toml`
     - `social.url`
     
   - `params.toml`
     - `footer.since`
  
     - `footer.launchDate`
  
   - `params.en.toml` | `params.zh.toml`
     - `favicon`(optional)
     - `footer.customText`
     - `sidebar.avatar`(optional)
     - `sidebar.emoji`(optional)
     - `sidebar.subtitle`
   
   其他目录修改：
   
   - `assets/img`

     头像图片等
   
   - `content/about`
   
     “关于”(About) 页面信息，内容改成自己的
   
   - 删除 `content/post/` 中的原有博文

## 其他修改(optional)

1. Hugo 版本升级涉及修改

   我当前使用的是 `0.165` 版本，部分设置项已被标记为 deprecated，建议更新为新设置项：

   - `config.toml`
     - `languageCode -> locale`

   - `languages.toml`
     - `languageCode -> locale`
     - `languageName -> label`
     - `languagedirection -> direction`

2. 关闭评论功能

   - `params.toml`

     - `comments.enabled = false`

     - `comments.waline.pageview = false`

     - 注释掉：`comments.waline.serverURL`

## 发布

修改完毕后提交（commit & push），等待一会后即可通过浏览器访问。
