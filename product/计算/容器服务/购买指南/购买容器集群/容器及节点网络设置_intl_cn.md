## 设置集群和节点网络

集群网络与容器网络是集群的基本属性。通过设置集群网络和容器网络可以规划集群的网络划分。
- **集群网络**：将为集群内主机分配在节点网络地址范围内的 IP 地址，您可以选择私有网络中的子网用于集群的节点网络，更多私有网络介绍可看 [私有网络和子网](https://intl.cloud.tencent.com/document/product/215/4927) 。
- **容器网络**：将为集群内容器分配在容器网络地址范围内的 IP 地址，您可以自定义三大私有网段作为容器网络， 根据您选择的集群内服务数量的上限，自动分配适当大小的 CIDR 段用于 kubernetes service；根据您选择 Pod 数量上限/节点，自动为集群内每台云服务器分配一个适当大小的网段用于该主机分配 Pod 的 IP 地址。

### 集群网络与容器网络的关系

- 集群网络和容器网络网段不能冲突
- 同一 VPC 内，不同集群的容器网络网段不能冲突
- 容器网络和 VPC 路由冲突时，优先在容器网络内转发

### 集群网络与腾讯云其他资源通信

- 集群内容器与容器之间互通
- 集群内容器与节点直接互通
- 集群内容器与 [云数据库 CDB](https://intl.cloud.tencent.com/product/cdb)、[云存储 Redis](https://intl.cloud.tencent.com/document/product/239/3205) 等资源同一 VPC 下内网互通

### 容器网络说明

1. 容器 CIDR：集群内 Sevice、 Pod 等资源所在网段
2. Services 数量上限/集群：决定分配给 Sevice 的 CIDR 大小
>? 腾讯云容器服务集群默认创建3个 Sevice：kubernetes、hpa-metrics-service、kube-dns，同时还会有2个广播地址和网络号，因此用户可以使用的是 serviceMax-5。
3. Pod 数量上限/Node：决定分配给每个 Node 的 CIDR 的大小
>? 腾讯云容器服务集群默认创建三个3个 Pod：kube-dns-xxxx、kube-dns-xxxx、l7-lb-controller-xxxx。
对于一个 Node 上的 Pod，有三个地址不能分配分别是网络号，广播地址和网关地址。故 Node 其最大的 Pod 数目=podMax-3
