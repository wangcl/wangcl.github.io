---
title: "How-To: Hugo Stack Starter"
description: Publish your personal homepage on GitHub Pages using the Hugo Stack Starter Template
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



## Operation Steps

1. Visit the [hugo-stack-starter](https://github.com/liu-houliang/hugo-stack-starter) repository in your browser (currently at `1.0.1 Release` version), and click the green <span style="color:#28a745;">Use this template</span> button at the top right.

2. When creating a new repository, name it `USER_NAME.github.io` (if you have an old version, recommend renaming the old repository for backup).

3. Enter the new repository, click Settings -> Pages，and change the `Source` to `GitHub Actions`

4. Modify the website framework content

   The files to be modified are mainly located in the `config/_default/` directory:

   - `config.toml`
     - `baseurl`: Must be changed to your personal homepage address `https://USER_NAME.github.io/`
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
   
   Other directories to modify:
   
   - `assets/img`

     Avatar images, etc.
   
   - `content/about`
   
     "About" page information, content should be changed to your own.
   
   - Delete the existing posts in `content/post/`

## Other Modifications (optional)

1. Hugo version upgrade involves modifications:

   I am currently using version `0.165`, some settings have been marked as **deprecated**. It is recommended to update to the new settings:

   - `config.toml`
     - `languageCode -> locale`

   - `languages.toml`
     - `languageCode -> locale`
     - `languageName -> label`
     - `languagedirection -> direction`

2. Disable comment functionality:

   - `params.toml`

     - `comments.enabled = false`

     - `comments.waline.pageview = false`

     - Comment out: `comments.waline.serverURL`

## Add New Posts

1. Create via Hugo command line:

   Execute the following in your local directory: `hugo new content post/link_path/index.zh.md`

   `link_path` is the subdirectory name under `content/post/`, which is also the last part of the published post URL.

2. Manually create:

   Enter the `content/post/` directory, manually create a subdirectory, and then add `index.zh.md` or `index.en.md` in it.

## Publishing

Execute `commit` & `push`, and wait a while for the website to be published. You can access it via your browser.

## Common Issues

1. GitHub Actions may occasionally enter a "ghost queue" state, causing the publish to fail. In this case, go to the **Actions** page on GitHub, click the **Build and deploy** workflow on the left, and manually handle the problematic task in the list on the right.

