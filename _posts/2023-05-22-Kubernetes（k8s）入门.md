---
title: Kubernetes（k8s）入门
author: mumu
date: 2023-05-22 11:00:00 +0800
categories: [Kubernetes,Kubernetes入门]
tags: [Kubernetes,k8s,Kubernetes入门]
pin: false
---

# Kubernetes入门[^1]

## 1  kubernetes 概述  

### 1.1. kubernetes 基本介绍  

1. Kubernetes 是 Google 开源的一个容器编排引擎  （2014年）
2. 使用k8s进行容器化应用部署
3. 使用k8s利于应用扩展
4. k8s目标实施让部署容器化应用更加简洁和高效

## 2 kubernetes 特性

### 2.1. 自动装箱

> 基于容器对应用运行环境的资源配置要求自动部署应用容器

### 2.2. 自我修复(自愈能力)

+ 当容器失败时， 会对容器进行重启
+ 当所部署的 Node 节点有问题时， 会对容器进行重新部署和重新调度
+ 当容器未通过监控检查时， 会关闭此容器直到容器正常运行时， 才会对外提供服务

### 2.3. 水平扩展

> 通过简单的命令、 用户 UI 界面或基于 CPU 等资源使用情况， 对应用容器进行规模扩大或规模剪裁

### 2.4. 服务发现

> 用户不需使用额外的服务发现机制， 就能够基于 Kubernetes 自身能力实现服务发现和负载均衡

### 2.5. 滚动更新

> 可以根据应用的变化， 对应用容器运行的应用， 进行一次性或批量式更新

### 2.6. 版本回退

> 可以根据应用部署情况， 对应用容器运行的应用， 进行历史版本即时回退

### 2.7. 密钥和配置管理

> 在不需要重新构建镜像的情况下， 可以部署和更新密钥和应用配置， 类似热部署。

### 2.8. 存储编排

> 自动实现存储系统挂载及应用， 特别对有状态应用实现数据持久化非常重要存储系统可以来自于本地目录、 网络存储(NFS、 Gluster、 Ceph 等)、 公共云存储服务

### 2.9. 批处理

> 提供一次性任务， 定时任务； 满足批量数据处理和分析的场景  

## 3 k8s 集群架构组件

### 3.1. Master（主控节点）和Node（工作节点）

#### 3.1.1. Master组件

![image-20230522224818952](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/04/20230522224826.png)

1. **<font color='red' style='background-color:' size=''>API Server</font>**：集群统一入口，以restful方式，交给etcd存储 
2. **<font color='red' style='background-color:' size=''>Scheduler</font>**  ：节点调度，选择Node节点应用部署
3. **<font color='red' style='background-color:' size=''>Controller MangerServer</font>**  ：处理集群中常规后台任务，一个资源对应一个控制器
4. **<font color='red' style='background-color:' size=''>ClusterState Store（ ETCD 数据库）</font>**   ：存储系统，用于保存集群相关的数据

#### 3.2.2. Node组件

![image-20230522225200347](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/04/20230522225200.png)

1. **<font color='red' style='background-color:' size=''>kubelet</font>**  ：master派到node节点的代表，管理本机容器
2. **<font color='red' style='background-color:' size=''>kube proxy</font>**  ：提供网络代理，负载均衡等操作

P4






























[^1]: [尚硅谷](http://www.atguigu.com/)