---
title: "How to Set Terminal in Sublime Text"
description: How to set Windows Terminal or Alacritty as default terminal for Sublime Text version 3213 or above.
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



## Problem Description

In Sublime Text version 4213, a right-click menu item `Open Terminal Here...` was added to the left-side sidebar for directories. It directly invokes the system's default terminal application, which is `cmd.exe` on Windows.

The default settings added a configuration item: `"terminal_command": "",` with examples in the comment:

```
// Examples:
// - "git-bash.exe --cd=\"$dir\""
// - "alacritty --working-directory \"$dir\""
"terminal_command": "",
```

The goal is to replace `cmd.exe` with modern terminals like Alacritty or Windows Terminal.

1. Format of configuration parameter

   Following the official configuration directly causes Alacritty to fail to start, with the error: `[ERROR] [alacritty] Invalid working directory: "\"D:\\Workspaces\\github\\notes\""`.

2. Alacritty starts correctly and opens the specified path. However, it also opens an extra cmd.exe window. Closing either window causes the other to close simultaneously.

## Problem Resolution

1. From the log, it's clear that the path parameter sent to Alacritty already includes double quotes. The `\"` in the configuration is redundant, causing the path recognition failure. Change the configuration to: `"terminal_command": "alacritty --working-directory $dir",`, which allows Alacritty to start normally and navigate to the specified path.

2. Change the configuration to: `"terminal_command": "cmd /c start /b alacritty --working-directory $dir",`, which closes the background `cmd.exe` window immediately, leaving only Alacritty.

> If using Windows Terminal instead of Alacritty, configure it as `"terminal_command": "wt -d $dir",` which makes the cmd window open and immediately close (flash briefly).

> Ensure the Alacritty (or Windows Terminal) path is added to the system's `PATH` environment variable.
