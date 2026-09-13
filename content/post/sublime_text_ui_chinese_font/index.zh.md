---
title: "Sublime Text 界面中文显示问题解决"
description: 修复 Sublime Text 界面（非编辑区）的中文显示问题
date: 2026-09-13T19:53:42+08:00
categories:
    - Tech
tags:
    - sublime
math: false
comments: false
draft: false
build:
    list: always
---

## 前言

一直用 Sublime Text 作为文本编辑器，从 2.x -> 3.x -> 4.x 版本。之前调整 Sublime Text 界面中文显示问题的方式是从 2.x、3.x 版本沿袭下来的，主要通过 `font_options` 设置 `gdi` 模式（可能还要增加 `dpi_scale` 限制字体放大），同时需要在操作系统的注册表中增加 `fontLink` 映射配置。

4.0 版本已经推出这么久了，把 `font_options` 从 `gdi` 调整成了 `gray_antialias` 后发现界面的中文显示还是和从前一样。以下通过覆盖默认配置的方式解决界面中文显示问题。

*终于可以删除注册表中那么多 `fontLink` 配置条目了，每次安装 windows 都要配置一遍。*

## 环境

- **操作系统：** windows 11
- **sublime text：** build_4200_x64 portable

## 修复步骤

1. 进入 Sublime Text 程序目录

2. 在 `Data/Packages/User/` 下增加两个文本文件：

   - `Default.sublime-theme`：浅色主题
   - `Default Dark.sublime-theme`：深色主题

3. 手动编辑配置信息（两个文件内容相同）：

   *中文使用 `Noto Sans Mono CJK SC` 仅仅是因为它有标准的英文名称*
   
   *使用时自行去掉注释部分*

```json
{
    "rules": 
    [
        // 标签页
        {
            "class": "tab_label",
            "font.face": "Noto Sans Mono CJK SC",
            "font.size": 12, 
        },

        // 侧边栏
        {
            "class": "sidebar_label",
            "font.face": "Noto Sans Mono CJK SC",
            "font.size": 12,
        },

        // ctrl + p 命令窗口
        {
            "class": "quick_panel_label",
            "font.face": "Noto Sans Mono CJK SC",
        },
        {
            "class": "quick_panel_path_label",
            "font.face": "Noto Sans Mono CJK SC",
        },

        // 如果 Preferences -> Settings 中的 font_size 设置增大，
        // 会导致 ctrl + f 搜索栏输入框的高度增大，超出其他元素。
        // 这里限制为小号字体，使其保持高度不变
        {
            "class": "text_line_control",
            "parents": [{"class": "panel_control"}],
            "font.size": 9,
        },

        // 状态栏 (optional)
        {
            "class": "label_control",
            "parents": [{"class": "status_bar"}],
            "font.face": "Consolas",
        },
    ]
}
```

