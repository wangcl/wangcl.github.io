---
title: "Maven 基础"
description: Maven 基础知识入门
date: 2015-03-10
categories:
    - Tech
tags:
    - maven
math: false
comments: false
draft: true
build:
    list: always
---

## 坐标

坐标是 Maven 构件的唯一标识。坐标由 `groupId`、`artifactId`、`version`、`packaging`、`classifier` 构成。

- `groupId`：组织的实际项目名，建议使用**组织的域名反向+项目名称**，如 `org.apache.maven.plugins`。
- `artifactId`：Maven 项目（模块）名，建议使用**实际项目名**作为模块的前缀，如 `maven-compiler-plugin`。
- `version`：Maven 项目的版本。版本号约定为 `<主版本>.<次版本>.<增量版本>-<里程碑版本>`，如 `1.2.3-beta-1`。Maven 还定义了 `SNAPSHOT` 快照版本，用于持续更新发布。
- `packaging`：Maven 项目的打包方式，通常与生成的构件的扩展名对应，如 `jar`，`war`。默认为 `jar`。
- `classifier`：用于帮助定义构建输出的一些附属构件。常见的使用场景如针对不同 JDK 版本（如 `jdk5`）。

坐标的5个元素中，`groupId`、`artifactId`、`version` 是必须指定的，`packaging` 可选（默认为 `jar`），`classifier` 是不能直接定义的（由辅助的插件帮助生成）。

Maven 构建出的文件的文件名一般为：`artifactId-version[-classifier].packaging`。
