---
title: GitLab Runner
author: mumu
date: 2023-04-18 18:15:00 +0800
categories: [GitLab,GitLab Runner]
tags: [GitLab,CI,CD,GitLab Runner]
pin: false
---

# GitLab Runner[^1]

## 1 GitLab Runner简介

### 1.1. GitLab Runner简介

+ GitLab Runner是一个开源项目，用于运行作业并将结果发送回GitLab。
+ 与GitLabCI结合使用，GitLabCI是GitLab随附的用于协调作业的开源持续集成服务。
+ GitLab Runner是用Go编写的，可以在Linux, macOS和Windows操作系统上运行。
+ 容器部署需使用最新Docker版本。GitLab Runner需要最少的Docker v1.13.0。.
+ GitLab Runner版本应与GitLab版本同步。（<font color='' style='background-color:yellow' size=''>避免版本不一致导致差异化</font>）。
+ 可以根据需要配置任意数量的Runner。

> GitLab Runner类似于Jenkins的agent ，执行CI持续集成、构建的脚本任务。

### 1.2. GitLab Runner特点

+ 作业运行控制: 同时执行多个作业。
+ 作业运行环境：
  + 在本地、使用Docker容器、使用Docker容器并通过SSH执行作业。
  + 使用Docker容器在不同的云和虚拟化管理程序上自动缩放。
  + 连接到远程SSH服务器。
+ 支持Bash，Windows Batch和Windows PowerShell。
+ 允许自定义作业运行环境。
+ 自动重新加载配置，无需重启。
+ 易于安装，可作为Linux，macOS和Windows的服务。

### 1.3. GitLab Runner类型与状态

+ 类型
  + shared 共享类型，运行整个平台项目的作业（gitlab）
  + group项目组类型，运行特定group下的所有项目的作业（group)
  + specific项目类型，运行指定的项目作业（project)
+ 状态
  + locked: 锁定状态，无法运行项目作业 
  + paused: 暂停状态，暂时不会接受新的作业

## 2 GitLab Runner安装

+ 系统环境: 可以在Linux，macOS，FreeBSD和Windows上安装和使用GitLab Runner 。
+ 部署方式: 二进制文件、rpm/deb软件包、Docker、Kubernetes。

## 3 GitLab Runner注册

## 4 GitLab Runner执行器

## 5 GitLab Runner命令

## 6 运行流水线任务



























[^1]: [GitLab的cicd自动发布构建流程-爱吃蓝猫的代码狗](https://www.bilibili.com/video/BV18y4y1S7VC/?spm_id_from=333.337.search-card.all.click&vd_source=95a54e0c2a8f9c16c6880629f2bed1af)