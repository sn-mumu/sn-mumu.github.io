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

   ![image-20230605190839754](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/04/20230605190847.png)

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
     192.168.8.11 k8smaster
     192.168.8.101 k8snode1
     192.168.8.102 k8snode2
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

     + docker

       1. 安装docker

          ```shell
          $ wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O/etc/yum.repos.d/docker-ce.repo
          $ yum -y install docker-ce-18.06.1.ce-3.el7
          $ systemctl enable docker && systemctl start docker
          $ docker --version
          ```

       2. 设置docker镜像加速器

          ```shell
          $ cat > /etc/docker/daemon.json << EOF
          {
          	"registry-mirrors": ["https://g41et2nr.mirror.aliyuncs.com"]
          } 
          EOF
          ```

     + 安装 kubeadm， kubelet 和 kubectl  

       1. 添加 yum 源 

          ```shell
          $ cat > /etc/yum.repos.d/kubernetes.repo << EOF
          [kubernetes]
          name=Kubernetes
          baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
          enabled=1
          gpgcheck=0
          repo_gpgcheck=0
          gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
          https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
          EOF
          ```

       2. 安装 kubeadm， kubelet 和 kubectl  

          ```shell
          $ yum install -y kubelet-1.18.0 kubeadm-1.18.0 kubectl-1.18.0
          $ systemctl enable kubelet
          ```

   + 部署 Kubernetes Master  

     1. 在master执行 `master`

        > *由于默认拉取镜像地址 k8s.gcr.io 国内无法访问， 这里指定阿里云镜像仓库地址*
        >
        > | apiserver          | 当前节点IP  |
        > | ------------------ | ----------- |
        > | image-repository   | 镜像仓库    |
        > | kubernetes-version | k8s版本     |
        > | service-cidr       | service子网 |
        > | pod-network-cidr   | pod子网     |

        ```shell
        $ kubeadm init \
        --apiserver-advertise-address=192.168.8.11 \
        --image-repository registry.aliyuncs.com/google_containers \
        --kubernetes-version v1.18.0 \
        --service-cidr=10.96.0.0/12 \
        --pod-network-cidr=10.244.0.0/16
        ```

     2. 使用 kubectl 工具 `master`

        ```shell
        $ mkdir -p $HOME/.kube
        $ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
        $ sudo chown $(id -u):$(id -g) $HOME/.kube/config
        $ kubectl get node	#查看node节点
        ```

     3. 安装 Pod 网络插件( **CNI** )`master`

        ```shell
        $ kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
        $ kubectl get pods -n kube-system
        NAME                                READY   STATUS    RESTARTS   AGE
        coredns-7ff77c879f-7nhpx            1/1     Running   0          9m58s
        coredns-7ff77c879f-j8cvb            1/1     Running   0          9m58s
        etcd-k8smaster                      1/1     Running   0          10m
        kube-apiserver-k8smaster            1/1     Running   0          10m
        kube-controller-manager-k8smaster   1/1     Running   0          10m
        kube-proxy-hjc7m                    1/1     Running   0          4m48s
        kube-proxy-k2h99                    1/1     Running   0          4m39s
        kube-proxy-zrvtj                    1/1     Running   0          9m58s
        kube-scheduler-k8smaster            1/1     Running   0          10m         
        ```

   + 加入 Kubernetes Node  

     1. 在node节点join 到master `node`

        > 默认token有效时间为24小时，当过期之后，该token就不可用了。这时就需要重新创建token
        >
        > ```shell
        > $ kubeadm token create --print-jion-command
        > ```

        ```shell
        $ kubeadm join 192.168.8.11:6443 --token gqicpf.wr12fuy0u0drtm6w \
            --discovery-token-ca-cert-hash sha256:b7ddb2cb04aae940f74ddf7ca70c359b3870ad8ffb88148f819499d4e308b8d1
        ```

   + 测试 kubernetes 集群  

     > 在 Kubernetes 集群中创建一个 pod， 验证是否正常运行  
     >
     > 访问地址： http://NodeIP:Port  

     ```shell
     $ kubectl create deployment nginx --image=nginx
     $ kubectl get pod
     NAME                    READY   STATUS    RESTARTS   AGE
     nginx-f89759699-v62mc   1/1     Running   0          45s
     $ kubectl expose deployment nginx --port=80 --type=NodePort
     $ kubectl get pod,svc
     NAME                        READY   STATUS    RESTARTS   AGE
     pod/nginx-f89759699-v62mc   1/1     Running   0          77s
     
     NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
     service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        15m
     service/nginx        NodePort    10.107.238.18   <none>        80:30688/TCP   11s
     ```


### 3.2. 二进制包  

> 从 github 下载发行版的二进制包， 手动部署每个组件， 组成Kubernetes 集群。  

#### 3.2.1. 安装要求

1. 一台或多台机器， 操作系统 CentOS7.x-86_x64
2. 硬件配置： 2GB 或更多 RAM， 2 个 CPU 或更多 CPU， 硬盘 30GB 或更多
3. 集群中所有机器之间网络互通
4. 可以访问外网， 需要拉取镜像， 如果服务器不能上网， 需要提前下载镜像并导入节点
5. 禁止 swap 分区  

#### 3.2.2. 准备环境

1. 软件环境

   | 软件       | 版本                    |
   | ---------- | ----------------------- |
   | 操作系统   | CentOS7.8_x64 （ mini） |
   | Docker     | 19-ce                   |
   | Kubernetes | 1.19                    |

2. 服务器规划

   | 角色       | IP            | 组件                                                         |
   | ---------- | ------------- | ------------------------------------------------------------ |
   | k8s-master | 192.168.8.12  | kube-apiserver， kube-controller-manager， kube -scheduler， etcd |
   | k8s-node1  | 192.168.8.111 | kubelet， kube-proxy， docker etcd                           |
   | k8s-node2  | 192.168.8.112 | kubelet， kube-proxy， docker etcd                           |

#### 3.2.3. 操作系统初始化配置

```shell
# 关闭防火墙
systemctl stop firewalld
systemctl disable firewalld
# 关闭 selinux
sed -i 's/enforcing/disabled/' /etc/selinux/config # 永久
setenforce 0 # 临时
# 关闭 swap
swapoff -a # 临时
sed -ri 's/.*swap.*/#&/' /etc/fstab # 永久
# 根据规划设置主机名
hostnamectl set-hostname <hostname>
# 在 master 添加 hosts
cat >> /etc/hosts << EOF
192.168.8.12 m1
192.168.8.111 n1
EOF
# 将桥接的 IPv4 流量传递到 iptables 的链
cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system # 生效
# 时间同步
yum install ntpdate -y
ntpdate time.windows.com
```

#### 3.2.4. 部署Etcd集群

#### 3.2.5. 安装docker

#### 3.2.6. 部署Master Node

### 3.3. 区别

> Kubeadm 降低部署门槛， 但屏蔽了很多细节， 遇到问题很难排查。
>
> 如果想更容易可控， 推荐使用二进制包部署 Kubernetes 集群， 虽然手动部署麻烦点， 期间可以学习很多工作原理， 也利于后期维护。  






























[^1]: [尚硅谷](http://www.atguigu.com/)