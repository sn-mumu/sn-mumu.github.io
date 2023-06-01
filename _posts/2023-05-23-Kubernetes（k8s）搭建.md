---
title: Kubernetes（k8s）搭建
author: mumu
date: 2023-05-23 22:30:00 +0800
categories: [Kubernetes,Kubernetes搭建]
tags: [Kubernetes,k8s,Kubernetes搭建]
pin: false
---

# Kubernetes搭建[^1]

## 1 搭建k8s环境平台规则

### 1.1. 单master集群

![image-20230523224331893](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/04/20230523224331.png)

### 1.2. 多master集群

![image-20230523224342111](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/04/20230523224342.png)

## 2 硬件服务器配置要求

### 2.1. 测试环境

1. master：2核 4G内存 20G磁盘
2. node：4核 8G内存 40G磁盘

## 3 搭建k8s集群的部署方式

### 3.1. [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm/  )  

> Kubeadm 是一个 K8s 部署工具， 提供 kubeadm init 和 kubeadm join， 用于快速部署 Kubernetes 集群。  

1. 安装3台centos7虚拟机

   + 2G-RAM 2-Core 30GB-Disk
   + 集群间网络之间互通
   + 可访问外网
   + 禁用swap分区

2. 对三台虚拟机进行初始化操作

   + 关闭防火墙

     ```shell
     $ systemctl stop firewalld
     $ systemctl disable firewalld  
     ```

   + 关闭selinux

     ```shell
     $ sed -i 's/enforcing/disabled/' /etc/selinux/config # 永久
     $ setenforce 0 # 临时
     ```

   + 关闭swap

     ```shell
     $ swapoff -a # 临时
     $ vim /etc/fstab # 永久
     ```

   + 配置主机名

     ```shell
     $ hostnamectl set-hostname <hostname>
     ```

   + 在 master 添加 hosts

     ```shell
     $ cat >> /etc/hosts << EOF
     192.168.88.201 k8smaster
     192.168.88.211 k8snode1
     192.168.88.212 k8snode2
     EOF

   + 将桥接的 IPv4 流量传递到 iptables 的链

     ```shell
     $ cat > /etc/sysctl.d/k8s.conf << EOF
     net.bridge.bridge-nf-call-ip6tables = 1
     net.bridge.bridge-nf-call-iptables = 1
     EOF
     $ sysctl --system # 生效
     ```

   + 时间同步  

     ```shell
     $ yum install ntpdate -y
     $ ntpdate time.windows.com
     ```

   + 所有节点安装 Docker/kubeadm/kubelet  

   + 部署 Kubernetes Master  

   + 安装 Pod 网络插件（ CNI）  

   + 加入 Kubernetes Node  

   + 测试 kubernetes 集群  

   

   

### 3.2. 二进制包  

> 从 github 下载发行版的二进制包， 手动部署每个组件， 组成Kubernetes 集群。  

### 3.3. 区别

> Kubeadm 降低部署门槛， 但屏蔽了很多细节， 遇到问题很难排查。
>
> 如果想更容易可控， 推荐使用二进制包部署 Kubernetes 集群， 虽然手动部署麻烦点， 期间可以学习很多工作原理， 也利于后期维护。  






























[^1]: [尚硅谷](http://www.atguigu.com/)