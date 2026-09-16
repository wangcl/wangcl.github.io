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

## 依赖

依赖的解决实际是 Maven 在 classpath 中添加相应的包。Maven 项目的依赖由 `pom.xml` 中的 `<dependency>` 元素设置。dependency 元素中常用的子元素包括：

- `groupId`、`artifactId`、`version`：依赖的基本坐标。
- `type`：依赖的类型，对应于坐标的 `packaging`。一般无需声明，默认为 `jar`。
- `scope`：依赖的范围。
- `optional`：是否为可选依赖。
- `exclusions`：用于排除传递性依赖。

### 依赖范围

![依赖范围](dep-scope.png)

其中 `compile` 是默认的依赖范围。

### 传递性依赖

项目声明依赖构件 A，而 A 声明依赖构件 B，则项目对构件 B 有传递性依赖。项目只需声明直接依赖，Maven 会通过传递性依赖机制自动处理间接依赖。

依赖范围对传递性依赖有影响：

![传递性依赖](transitive-dep.png)

### 依赖调解

依赖调解用于解决依赖的冲突。例如项目依赖了构件 A、B，A 和 B 都间接依赖了 X，但 A 依赖了 X 的 1.0 版本，B 依赖了 X 的 2.0 版本，则产生了依赖冲突。

Maven 依赖调解的原则：

1. **最短路径优先**：传递的路径短者优先。如 A 对 X 的 1.0 版本的依赖路径为 A -> C -> X（1.0），B 对 X 的 2.0 版本的依赖路径为 B -> D -> E -> X（2.0），由于 X 1.0 的路径长度（2） < X 2.0 的路径长度（3），则最终依赖为 X 1.0。
2. **先声明者优先**：如果路径相同，则使用声明在前的依赖。

### 可选依赖

如果依赖的 `optional` 元素设置为 `true`，则不会产生传递性依赖。如项目 A 依赖 B，B 依赖 X（可选），则 A 不依赖于 X。

可选依赖的使用场景一般为某个构件提供了多种实现，需根据情况显式选择一种。例如提供了基于 Oracle、MySQL 等多种持久层实现时，根据采用的数据库选择对应的实现。

### 排除依赖

排除依赖显式的解除某传递性依赖。一般用于替换某传递性依赖的版本。例如 A 依赖 B，B 依赖 X 的版本 1.0。可将 B 对 X 的依赖通过 `exclusion` 元素排除，然后显式指定 A 依赖 X 的 1.0.1 版本（此场景虽然可以通过依赖调解实现，但通过显式指定使用的版本看起来更清晰）。

`exclusion` 元素中只需 `groupId` 和 `artifactId` 即可，无需 `version` 元素。

## 生命周期和插件

### 生命周期

Maven 为项目的构建过程抽象了统一的生命周期，涵盖了项目的所有构建步骤。

Maven 内置了 3 套相互独立的生命周期：`clean`、`default`、`site`，每个周期又分为多个阶段（`phase`）。周期的阶段是有顺序的，后边的阶段的执行依赖之前的阶段，即执行后边阶段时 Maven 会自动依次执行之前的阶段。

#### clean 生命周期

clean 生命周期主要做清理项目的工作。

- pre-clean
- clean
- post-clean

#### default 生命周期

default 生命周期定义了真正构建时需要执行的所有步骤。

- validate
- initialize
- generate-sources
- process-sources
- generate-resources
- process-resources
- compile
- process-classes
- generate-test-sources
- generate-test-resources
- process-test-resources
- test-compile
- process-test-classes
- test
- prepare-package
- package
- pre-integration-test
- integration-test
- post-integration-test
- verify
- install
- deploy

#### site 生命周期

- pre-site
- site
- post-site
- site-deploy

#### 命令行与生命周期

常用的 Maven 命令实际调用了 Maven 的生命周期阶段。如：

- `mvn clean` 命令：调用 clean 生命周期的 clean 阶段（实际执行 pre-clean 和 clean 阶段）
- `mvn test` 命令：调用 default 生命周期的 test 阶段（实际执行 validate -> test 的所有阶段）
- `mvn clean install` 命令：调用 clean 生命周期的 clean 阶段 + default 生命周期的 install 阶段
- `mvn clean deploy site-deploy` 命令：调用 clean 生命周期的 clean 阶段 + default 生命周期的 deploy 阶段 + site 生命周期的 site-deploy 阶段

### 插件

Maven 的核心仅仅定义了抽象的生命周期，而实际的功能是通过插件实现的。插件以独立的 Maven 构件的形式存在，在需要时 Maven 自动从仓库下载。每个 Maven 的插件实现一类功能，每个功能是一个插件目标（Goal）。

Maven 插件的目标需要与指定的生命周期的阶段相绑定，用以完成具体的构建任务。

#### 内置绑定

Maven 内置了很多绑定，从而默认可以执行大部分的构建功能：

clean 生命周期：

- clean 阶段：`maven-clean-plugin:clean`

default 生命周期：

- process-resources 阶段：`maven-resources-plugin:resources`
- compile 阶段：`maven-compiler-plugin:compile`
- process-test-resources 阶段：`maven-resources-plugin:testResources`
- test-compile 阶段：`maven-compiler-plugin:testCompile`
- test 阶段：`maven-surefire-plugin:test`
- package 阶段：`maven-jar-plugin:jar`
- install 阶段：`maven-install-plugin:install`
- deploy 阶段：`maven-deploy-plugin:deploy`

site 生命周期：

- site 阶段：`maven-site-plugin:site`
- site-deploy 阶段：`maven-site-plugin:deploy`

#### 自定义绑定

