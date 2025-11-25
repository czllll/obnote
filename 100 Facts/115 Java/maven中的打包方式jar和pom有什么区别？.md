---
tags:
- java devops
- maven
---
1. jar 打包方式：

- 用于普通的 Java 项目模块
- 会将项目的代码、资源文件等打包成一个可执行或可引用的 JAR 文件
- 打包后的 JAR 包含：.class 文件、配置文件、资源文件等
- 通常用于具体的功能模块实现，比如你的 mockredbook-auth 模块

1. pom 打包方式：

- 专门用于父项目 (parent project) 或聚合项目 (aggregator project)
- 不会打包任何代码或资源文件
- 仅用于管理子模块、依赖和插件
- 主要职责是：
  - 统一管理依赖版本
  - 聚合管理多个子模块
  - 继承配置给子模块