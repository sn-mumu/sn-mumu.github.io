---
title: GitLab CI/CD基本介绍
author: mumu
date: 2023-04-18 16:40:00 +0800
categories: [GitLab,GitLab CI/CD基本介绍]
tags: [GitLab,CI,CD]
pin: false
---

# GitLab CI/CD基本介绍[^1]

## 1 为什么要做CID/CD？

### 1.1. 传统应用发布模式

***弊端：***

1. 错误发现不及时
2. 人工低级错误发生
3. 团队工作效率低
4. 开发运维对立

![image-20230418165841103](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/04/202304181658200.png)



## 1.2. CI/CD

### 1.2.1. 持续集成（Continuous Integration，*<font color='red' style='background-color:' size=''>CI</font>*）

+ 合并开发人员正在开发编写的所有代码的一种做法。
+ 通常一天内进行多次合并和提交代码。
+ 从存储库或生产环境中进行构建和自动化测试，以确保没有集成问题并及早发现任何问题。

**优点：**

+ 尽快发现错误
+ 减少集成问题
+ 避免复杂问题

### 1.2.2. 连续交付（Continuous Deployment，*<font color='red' style='background-color:' size=''>CD</font>*）

+ 通常可以通过将更改自动推送到发布系统来随时将软件发布到生产环境中。
+ 持续部署会更进一步，并自动将更改推送到生产中。

**优点：**

+ 每次更改都可发布
+ 降低每次发布风险
+ 更加频繁交付价值
+ 快速频繁客户反馈

## 2 持续集成-Jenkins

### 2.1. 优点

+ 可扩展自动化服务器
+ 安装配置简单
+ 丰富的插件库
+ 分布式架构设计
+ 支持所有的平台
+ 可视化的管理页面

## 3 代码版本管理-GitLab

+ 代码审查
+ 问题跟踪
+ 动态订阅
+ 易于扩展
+ 项目wiki
+ 多角色项目管理
+ 项目代码在线预览
+ CI工具集成

## 4 GitLabCI/CD优势

+ 开源:CI/CD是开源GitLab社区版和专有GitLab企业版的一部分。
+ 易于学习:具有详细的入门文档。
+ 无缝集成: GitLab CI / CD是GitLab的一部分，支持从计划到部署,具有出色的用户体验。
+ 可扩展:测试可以在单独的计算机上分布式运行，可以根据需要添加任意数量的计算机。
+ 更快的结果:每个构建可以拆分为多个作业，这些作业可以在多台计算机上并行运行。
+ 针对交付进行了优化:多个阶段，手动部署，环境和变量。

## 5 GitLab CI/CD特点

+ 多平台:Unix，Windows，macOS和任何其他支持Go的平台上执行构建。
+ 多语言:构建脚本是命令行驱动的，并且可以与Java，PHP，Ruby，C和任何其他语言一起使用。
+ 稳定构建:构建在与GitLab不同的机器上运行。
+ 并行构建:GitLab CI/ CD在多台机器上拆分构建，以实现快速执行。
+ 实时日志记录:合并请求中的链接将您带到动态更新的当前构建日志。
+ 灵活的管道:您可以在每个阶段定义多个并行作业，并且可以触发其他构建。
+ 版本管道:一个`.gitlab-ci.yml`文件包含您的测试，整个过程的步骤，使每个人都能贡献更改，并确保每个分支获得所需的管道。
+ 自动缩放:您可以自动缩放构建机器，以确保立即处理您的构建并将成本降至最低。
+ 构建工件:您可以将二进制文件和其他构建工件上载到 GitLab并浏览和下载它们。
+ Docker支持:可以使用自定义Docker映像，作为测试的一部分启动服务，构建新的Docker映像，甚至可以在Kubernetes上运行。
+ 容器注册表:内置的容器注册表，用于存储，共享和使用容器映像。
+ 受保护的变量:在部署期间使用受每个环境保护的变量安全地存储和使用机密。
+ 环境:定义多个环境。

## 6 GitLab CI/CD 组件

![image-20230418174826554](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/04/202304181748613.png)

+ GitLab CI / CD
  + GitLab的一部分，GitLab是一个Web应用程序，具有将其状态存储在数据库中的API。
  + 除了GitLab的所有功能之外，它还管理项目/构建并提供一个不错的用户界面。
+ GitLab Runner
  + 是一个处理构建的应用程序。
  + 它可以单独部署，并通过API与GitLab CI / CD一起使用。
+ `. gitlab-ci.ymlI`
+ 为了运行测试，至少需要一个GitLab 实例和一个GitLabRunner。

## 7 GitLab CI/CD工作原理

+ 将代码托管到Git存储库
+ 在项目根目录创建ci文件 `.gitlab-ci.yml` ，在文件中指定构建，测试和部署脚本。
+ GitLab将检测到它并使用名为GitLab Runner的工具运行脚本。
+ 脚本被分组为**作业**，它们共同组成了一个**管道**。



[^1]: [GitLab的cicd自动发布构建流程-爱吃蓝猫的代码狗](https://www.bilibili.com/video/BV18y4y1S7VC/?spm_id_from=333.337.search-card.all.click&vd_source=95a54e0c2a8f9c16c6880629f2bed1af)
