---
title: "Windows 11 Ltsc 24H2 手动安装 WSL 2"
description: 描述如何在 windows 11 LTSC 无 Microsoft Store / winget 时手动安装 WSL 2
date: 2026-10-07T15:07:05+08:00
lastmod: 
categories:
    - Tech
tags:
    - wsl
math: false
comments: false
draft: false
build:
    list: always
---

## Windows 11 LTSC 24H2 无 Microsoft Store / winget 安装 WSL 2 手册

> 适用环境：Windows 11 LTSC 24H2\
> 前提：系统没有 Microsoft Store，也没有 `winget`\
> 目标：安装 **WSL 2 + Ubuntu 24.04 LTS**，并完成验证

----------

### 1. 方案概览

Windows 11 LTSC 不需要 Microsoft Store 或 `winget` 才能使用 WSL 2。

对于当前的 Windows 11 LTSC 24H2，采用微软现在提供的**离线/手动安装路线**：

1.  确认 Windows 版本和 CPU 架构
2.  从 Microsoft 官方 GitHub 下载最新 WSL MSI
3.  用 DISM 启用 `VirtualMachinePlatform`
4.  重启 Windows
5.  安装 WSL MSI
6.  下载 Ubuntu 24.04 LTS 的 `.wsl` 发行版包
7.  使用 `wsl --install --from-file` 或直接双击 `.wsl` 安装发行版
8.  将默认 WSL 版本设置为 2
9.  创建 Linux 用户
10. 用 `wsl --status`、`wsl -l -v` 验证

### 2. 先确认 Windows 11 LTSC 24H2

`按：

``` text
Win + R
````

输入：

``` text
winver
```

应看到类似：

``` text
Windows 11
Version 24H2
OS Build 26100.xxxx
```

Windows 11 本身已经满足 WSL 2 的系统版本要求。

----------

### 3. 确认 CPU 架构

在 PowerShell 中执行：

``` powershell
$env:PROCESSOR_ARCHITECTURE
```

普通 Intel / AMD Windows 11 电脑应该返回：

``` text
AMD64
```

也可以执行：

``` powershell
systeminfo | findstr /C:"System Type"
```

如果显示：

``` text
System Type:               x64-based PC
```

则下载 **x64 / x64.msi** 版本。

> 本文后续命令以 x64 Windows 为例。

----------

### 4. 检查 BIOS / UEFI 虚拟化

WSL 2 依赖硬件虚拟化。

打开：

``` text
任务管理器 → 性能 → CPU
```

查看：

``` text
虚拟化：已启用
```

如果显示：

``` text
虚拟化：已禁用
```

需要进入 BIOS / UEFI，开启：

-   Intel CPU：Intel Virtualization Technology / VT-x
-   AMD CPU：SVM / AMD-V

----------

### 5. 下载最新 WSL MSI

#### 5.1 官方下载位置

打开 Microsoft 官方 WSL Releases：

https://github.com/microsoft/WSL/releases

选择最新的**稳定版（Latest，而不是 Pre-release）**。

在 Assets 中找到类似：

``` text
wsl.x.x.x.x.x64.msi
```

或者当前版本对应的：

``` text
wsl.<version>.0.x64.msi
```

下载 x64 MSI。

### 6. 以管理员身份打开 PowerShell

右键开始菜单：

``` text
Windows PowerShell (管理员)
```

后面的 DISM 命令必须在管理员权限下执行。

----------

### 7. 启用 Virtual Machine Platform

在管理员 PowerShell 中执行：

``` powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

正常情况下最后会看到类似：

``` text
The operation completed successfully.
```

中文系统可能显示：

``` text
操作成功完成。
```

然后检查功能状态：

``` powershell
dism.exe /online /get-featureinfo /featurename:VirtualMachinePlatform
```

应该看到：

``` text
State : Enabled
```

----------

### 8. 是否还需要启用 Windows Subsystem for Linux 功能？

这里需要特别区分两种 WSL 安装方式。

#### 推荐的现代 WSL MSI 路线

微软当前的离线安装说明要求：

-   安装 WSL MSI
-   启用 Virtual Machine Platform
-   安装 `.wsl` 发行版

因此，使用现代 WSL MSI 时，不必把旧版"WSL optional component + Linux
kernel MSI"的流程作为主流程。

#### 如果系统提示 WSL optional component 未启用

可以额外执行：

``` powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

然后检查：

``` powershell
dism.exe /online /get-featureinfo /featurename:Microsoft-Windows-Subsystem-Linux
```

应该为：

``` text
State : Enabled
```

> 对于 LTSC，如果后面出现与 WSL optional component 有关的兼容性错误，这一步可以作为排障措施。

----------

### 9. 重启 Windows

一定重启，不要跳过。

----------

### 10. 安装 WSL MSI

Windows 重启后，找到刚才下载的：

``` text
wsl.x.x.x.x.x64.msi
```

直接双击安装。

如果 Windows 弹出 UAC：

``` text
是否允许此应用对你的设备进行更改？
```

选择：

``` text
是
```

安装完成后关闭安装程序。

----------

### 11. 检查 WSL 是否安装成功

打开普通 PowerShell，执行：

``` powershell
wsl --version
```

现代 WSL 应该显示类似：

``` text
WSL version: 2.x.x
Kernel version: ...
WSLg version: ...
MSRDC version: ...
Direct3D version: ...
DXCore version: ...
Windows version: ...
```

> 版本号会随着 WSL 发布而变化。

如果：

``` powershell
wsl --version
```

能够正常显示 WSL 版本信息，说明现代 WSL 包已经安装。

----------

# 12. 设置默认 WSL 版本为 2

执行：

``` powershell
wsl --set-default-version 2
```

正常情况下会显示：

``` text
The operation completed successfully.
```

或者中文提示成功。

以后新安装的 Linux 发行版默认使用 WSL 2。

----------

### 13. 安装 Ubuntu 24.04 LTS

由于没有 Microsoft Store，直接使用 `.wsl` 文件安装。

微软当前的离线安装文档建议通过 WSL 的发行版信息下载 `.wsl` 包。

官方发行版信息：

https://github.com/microsoft/WSL/blob/master/distributions/DistributionInfo.json

在其中找到 Ubuntu 24.04 LTS 对应的 x64 下载地址。

----------

#### 13.1 推荐：使用 `.wsl` 文件

下载后，例如：

``` text
D:\Downloads\WSL\ubuntu-24.04.wsl
```

然后执行：

``` powershell
wsl --install --from-file D:\Downloads\WSL\ubuntu-24.04.wsl
```

如果当前 WSL 版本支持该命令，这是推荐方式。

也可以尝试直接双击 `.wsl` 文件。

首次启动 Ubuntu 时，会要求：

``` text
Enter new UNIX username:
```

输入一个 Linux 用户名，例如：

``` text
somebody
```

然后设置密码。

----------

### 14. 如果 `wsl --install --from-file` 不支持

某些 WSL 版本或特定安装状态下可能没有该参数。

先查看：

``` powershell
wsl --help
```

如果没有：

``` text
--from-file
```

可以采用 `.appx` / `.AppxBundle` 手动安装方式。

微软官方手动安装文档仍然提供了这种方式。

----------

#### 14.1 下载 Ubuntu 24.04 LTS AppxBundle

微软文档提供了 Ubuntu 24.04 LTS 的直接下载方式。

例如可以使用：

``` powershell
curl.exe -LR -o D:\Downloads\WSL\ubuntu-2404.AppxBundle "https://wslstorestorage.blob.core.windows.net/wslblob/Ubuntu2404-240425.AppxBundle"
```

> 如果微软后续更新了 Ubuntu 24.04 LTS 的包文件名，应以官方 WSL DistributionInfo.json 中的当前下载地址为准，不要长期依赖本文示例文件名。

----------

#### 14.2 使用 Add-AppxPackage 安装

进入下载目录：

``` powershell
cd D:\Downloads\WSL
```

执行：

``` powershell
Add-AppxPackage .\ubuntu-2404.AppxBundle
```

如果文件实际名称不同，把命令中的文件名替换成实际名称。

安装完成后，可以从开始菜单启动 Ubuntu。

也可以在 PowerShell 执行：

``` powershell
wsl -l
```

应该看到 Ubuntu。

### 15. 第一次启动 Ubuntu

第一次启动 Ubuntu
可能需要几十秒甚至更长时间，因为系统正在解包并初始化发行版。

随后会看到：

``` text
Enter new UNIX username:
```

例如：

``` text
somebody
```

然后：

``` text
New password:
```

输入 Linux 密码。

再次输入：

``` text
Retype new password:
```

完成后进入：

``` text
somebody@WindowsHost:~$
```

看到类似提示，就说明 Ubuntu 已经启动。

----------

### 16. 验证 Ubuntu 是否真的运行在 WSL 2

回到 Windows PowerShell：

``` powershell
wsl -l -v
```

应该看到类似：

``` text
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

最重要的是最后一列：

``` text
VERSION
2
```

必须是：

``` text
2
```

而不是：

``` text
1
```

----------

### 17. 查看 WSL 总体状态

执行：

``` powershell
wsl --status
```

可以查看：

-   默认发行版
-   默认 WSL 版本
-   Kernel
-   相关配置

还可以：

``` powershell
wsl --version
```

查看：

-   WSL 版本
-   Linux kernel
-   WSLg
-   Windows 版本

----------

### 18. 在 Ubuntu 中验证 Linux 环境

进入 Ubuntu：

``` powershell
wsl
```

然后执行：

``` bash
uname -a
```

再执行：

``` bash
cat /etc/os-release
```

Ubuntu 24.04 应显示类似：

``` text
VERSION_ID="24.04"
```

再执行：

``` bash
uname -r
```

如果看到包含：

``` text
microsoft-standard-WSL2
```

等 WSL2 标识，则说明 Linux 内核运行在 WSL 2 环境。

----------

### 19. 验证 Windows 与 WSL 文件互通

在 Ubuntu 中：

``` bash
ls /mnt/c
```

应该能看到 Windows C 盘内容。

例如：

``` bash
ls /mnt/d
```

可以访问 D 盘。

Windows 文件：

``` text
D:\Apps
```

对应：

``` text
/mnt/d/Apps
```

因此你的 Windows 开发环境可以与 WSL Linux 环境方便地共享文件。

----------

### 20. 从 Windows 启动指定发行版

查看发行版：

``` powershell
wsl -l
```

启动 Ubuntu：

``` powershell
wsl -d Ubuntu
```

如果发行版名称不同，以：

``` powershell
wsl -l
```

显示的名称为准。

----------

### 21. 设置 Ubuntu 为默认发行版

如果以后安装了多个发行版，可以：

``` powershell
wsl --set-default Ubuntu
```

然后：

``` powershell
wsl
```

就会直接进入 Ubuntu。

----------

### 22. 关闭 WSL

关闭所有 WSL：

``` powershell
wsl --shutdown
```

这会终止正在运行的 WSL 2 虚拟机。

如果只想关闭 Ubuntu：

``` powershell
wsl --terminate Ubuntu
```

----------

### 23. 常用管理命令

  | 操作                   | 命令                          |
  | ---------------------- | ----------------------------- |
  | 查看 WSL 版本          | `wsl --version`               |
  | 查看 WSL 状态          | `wsl --status`                |
  | 查看发行版             | `wsl -l`                      |
  | 查看发行版及 WSL 版本  | `wsl -l -v`                   |
  | 启动默认发行版         | `wsl`                         |
  | 启动指定发行版         | `wsl -d Ubuntu`               |
  | 设置默认发行版         | `wsl --set-default Ubuntu`    |
  | 设置默认 WSL 2         | `wsl --set-default-version 2` |
  | 关闭所有 WSL           | `wsl --shutdown`              |
  | 关闭一个发行版         | `wsl --terminate Ubuntu`      |
  | 将发行版转换为 WSL 2   | `wsl --set-version Ubuntu 2`  |

----------

### 24. 如果 Ubuntu 显示 VERSION = 1

执行：

``` powershell
wsl --set-version Ubuntu 2
```

然后检查：

``` powershell
wsl -l -v
```

确认：

``` text
Ubuntu    Running    2
```

转换过程可能需要一些时间。

----------

### 25. 常见问题排查

#### 25.1 `wsl` 命令不存在

如果出现：

``` text
'wsl' is not recognized...
```

首先确认 WSL MSI 是否安装成功。

执行：

``` powershell
where.exe wsl
```

正常情况下应能找到：

``` text
C:\Windows\System32\wsl.exe
```

然后重新打开 PowerShell。

如果仍然不存在，检查：

``` powershell
dism.exe /online /get-featureinfo /featurename:VirtualMachinePlatform
```

以及：

``` powershell
dism.exe /online /get-featureinfo /featurename:Microsoft-Windows-Subsystem-Linux
```

----------

### 26. `wsl --install` 不适合当前安装环境怎么办？

不要把：

``` powershell
wsl --install
```

作为 LTSC + 无 Store 环境的唯一方案。

微软目前已经提供独立 WSL MSI 和离线发行版安装方案。

因此推荐顺序是：

``` text
WSL MSI
   ↓
Virtual Machine Platform
   ↓
重启
   ↓
Ubuntu .wsl
   ↓
wsl -l -v
```

而不是依赖：

``` text
Microsoft Store
   ↓
winget
   ↓
wsl --install
```

----------

### 27. `wsl --update` 是否必须？

如果使用现代 WSL MSI，通常不需要依赖 Microsoft Store 更新 WSL。

可以查看：

``` powershell
wsl --version
```

如果已经是当前安装的 WSL 版本，就无需为了安装 WSL 2 再单独下载旧式 Linux
kernel MSI。
