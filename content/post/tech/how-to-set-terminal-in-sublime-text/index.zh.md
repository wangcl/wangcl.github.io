---
title: "设置 Sublime Text 中的终端应用"
description: 描述如何将 Sublime Text (3213+) 中的终端应用绑定到 Windows Terminal 或者 Alacritty
date: 2026-10-03T14:33:03+08:00
lastmod: 
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



## 问题描述

sublimetext 在 4213 版本中，为界面左侧的 side bar 中的目录增加了一个鼠标右键菜单：`Open Terminal Here...`。直接调用系统默认终端应用，windows 下是 `cmd.exe`。

默认的 settings 中增加了一个配置项：`"terminal_command": "",`，并在注释中给出了示例：

```
// Examples:
// - "git-bash.exe --cd=\"$dir\""
// - "alacritty --working-directory \"$dir\""
"terminal_command": "",
```

将 `cmd.exe` 替换为现代化的终端应用，如 `alacritty` 或 `windows terminal`。

1. 配置参数格式

   直接按照官方配置，alacritty 启动错误，提示：`[ERROR] [alacritty] Invalid working directory: "\"D:\\Workspaces\\github\\notes\""`。

2. alacritty 正常启动，并打开了指定路径。但是额外打开了一个 cmd.exe 窗口，关闭任意一个，另一个也会同时关闭

## 问题修复

1. 从日志信息中可以看出，路径参数发送给 alacritty 时已经自带了双引号，配置中的 `\"` 冗余，导致路径识别错误。将配置改为：`"terminal_command": "alacritty --working-directory $dir",`，alacritty 正常启动并位于指定路径。

2. 将配置改为：`"terminal_command": "cmd /c start /b alacritty --working-directory $dir",`

   后台的 cmd.exe 窗口立即自动关闭，达到只保留 alacritty 的效果。

> 如果不使用 alacritty，而是用 windows terminal，那么配置成 `"terminal_command": "wt -d $dir",`，cmd 窗口打开后会自动关闭（一闪而逝）

> 需要将 alacritty | windows terminal 路径配置到环境变量 PATH 中。
