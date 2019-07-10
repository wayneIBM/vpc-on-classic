---
copyright:
  years: 2017, 2019
lastupdated: "2019-05-29"
keywords: features, benefits, isolation, provisioning, security, cloud-native, workloads, BYOIP, vpc

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# 关于 Virtual Private Cloud
{: #about}

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) 是下一代 IBM Cloud 平台。通过 VPC，您可以在 IBM Cloud 中创建自己的空间，以在公共云中运行隔离环境。VPC 让您既拥有私有云的安全性，也具备公共云的敏捷性和易用性。

可以根据需要管理网络服务和启动实例，以支持任务关键型应用程序、云容错应用程序和云本机应用程序。可用的主要功能包括：

* 通过 API 来创建和管理隔离的应用程序环境
* 定义您自己的联网策略，以实现安全性和方便访问
* 设计使用 BYOIP（自带 IP）的网络拓扑
* 供应资源并将其相互连接，也可以将资源相互隔离
* 覆盖多个区域，以实现灾难恢复和弹性
* 使用可用性专区，通过 HA，允许跨区域建立等待时间短的高速连接
* 利用高速联网和存储设备
* 允许永远可用服务（控制平面）
* 提供并利用核心服务：IPAM、VPN、防火墙、SSH、DNS 和 L4 负载均衡
* 设置其他服务：自动缩放、CDN 和第三方 NFV

下面的“VPC 概览图”演示了可用的 VPC 组件以及这些组件如何协作来为您提供可能需要的服务和连接。在深入了解提供每个组件更多相关详细信息的主题时，可回顾此图。

![IBM Cloud VPC 概览图](images/vpc-experience-simple.svg "IBM Cloud VPC 概览图")

以下各部分提供了 IBM Cloud VPC 中可用功能的概述。

## 联网功能

{{site.data.keyword.cloud_notm}} VPC 提供了全面、易用的联网功能，包括能够创建[全球可用的多专区区域](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region)中的多个虚拟私有云、不同专区中的子网、[IP 地址范围选择](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)、虚拟防火墙（[安全组](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)和[网络 ACL](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls)）、站点到站点虚拟专用网 ([VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)) 以及具有弹性的负载均衡 ([LBaaS](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc))。

VPC 划分成子网，并使用一系列专用 IP 地址。不过，缺省情况下，同一 VPC 中的所有资源（如 VSI）都可以相互通信，而与其子网无关。子网包含在单个专区中，并且子网不能跨多个专区，这有助于缩短等待时间，还有利于安全性和高可用性。

使用建议的前缀范围和预配置的网络安全策略，可轻松创建多个 VPC 和子网，或者设计并定义您自己的地址前缀和定制安全策略。

CIDR 块 161.26/16 和 166.8/14 都保留并路由到每个子网中。请阅读有关[可用于 IBM Cloud VPC 的服务端点](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)的信息。此外，CIDR 块 10.240/13 保留用于 VPC 地址前缀，并[以预定义方式](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#ibm-cloud-vpc-and-address-prefixes)在专区之间进行分配，无法对此块进行更改。
{: important}

### 因特网访问权

有三个选项可用于启用从虚拟服务器实例 (VSI) 到公用因特网的通信：
* 使用公共网关 (PGW) 支持连接的子网上所有虚拟服务器实例到因特网的通信。除了所用带宽外，使用 PGW 没有其他任何费用。
* 使用浮动 IP (FIP) 支持单个虚拟服务器实例 (VSI) 与因特网之间的进出通信。
* 使用 VPN 网关。有关更多信息，请参阅 [VPN for VPC 文档](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc#--using-vpn-with-your-vpc)。
{: note}

### 经典访问

现有 {{site.data.keyword.cloud_notm}} 基础架构用户可以使用[经典访问](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)将其经典基础架构（包括 Direct Link 连接）连接到每个区域中的一个 VPC。

### BYOIP

您可以在 IBM Cloud VPC 帐户中使用自带公共 IPv4 地址范围 (BYOIP)。

使用 BYOIP 时，{{site.data.keyword.cloud_notm}} 必须在 {{site.data.keyword.cloud_notm}} 资源上配置这些 IPv4 地址，这将与您提供的地址之间进行收发包操作。因此，在 {{site.data.keyword.cloud_notm}} 上使用您提供的 IPv4 范围后，这些 IP 地址可能会向 IBM 的支持人员和第三方公开。
{: important}

必须通过 API 或 CLI 来使用 BYOIP。此功能在 IBM Cloud 控制台 UI 中不可用。
{: note}

### 全球连接

可以将应用程序和可用资源的作用域限定为跨多个区域。通过使用 [VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)，可以创建与其他项目以及与混合云部署中其他部分的专用连接。

可轻松缩放且可共享，VPC 网络和资源可从内部部署设施延伸到云中。

## 安全性

在 {{site.data.keyword.cloud_notm}} VPC 中，通过[安全组](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)和[网络访问控制表](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) (ACL) 集成了安全性；其中，安全组充当虚拟防火墙，提供实例级别保护，而 ACL 提供子网级别保护。

## 高可用性

通过 {{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC)，应用程序可与所有区域中的其他网络进行逻辑隔离，同时提供可伸缩性和安全性。使用[负载均衡器](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)可在一组目标之间分发网络流量，以提高性能和 HA。负载均衡器还可监视应用程序和服务的运行状况。您可以设置负载均衡器，以跨单个专区中的各个实例或跨一个区域内的多个专区分配入局应用程序流量。

## 计算功能

使用 [IBM Cloud Virtual Servers for Virtual Private Cloud](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-virtual-private-cloud) 可在 IBM Cloud 中供应可缩放计算资源。使用针对特定工作负载优化的预定义[概要文件](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles)，可快速创建虚拟服务器实例 (VSI)。供应多宿主、[多 vNIC](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-network-security-options) 实例时，请从支持的库存[映像](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-images)中进行选择或上传定制映像。

{{site.data.keyword.cloud_notm}} VPC 添加了一个网络编排层，用于消除虚拟服务器实例的 pod 边界，从而为缩放实例创建无限容量。网络编排层处理不同区域和专区的 IBM Cloud VPC 中所有虚拟服务器实例的所有联网。通过 {{site.data.keyword.cloud_notm}} VPC 提供的软件定义联网功能，您有 VPN、LBaaS 和多 vNIC 实例的更多选项可选择，并且能使用更大的子网大小。

## 存储器功能

供应 {{site.data.keyword.cloud_notm}} Virtual Servers for Virtual Private Cloud 实例时，会创建一个 100 GB 的通用 IOPS (3 IOPS/GB) 块存储卷作为主引导卷，并连接到该实例。在 VPC 网络中供应虚拟服务器实例或创建独立于 VSI 生命周期的新卷时，可以创建块存储卷。

为 VPC 供应更多块存储器时，可以选择用于块存储卷的 [IOPS 层](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#tiers)，也可以指定[定制 IOPS 概要文件](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#custom)。

## 针对云本机和混合工作负载设计

VPC 是云本机工作负载的理想选择，用于将现有基础架构链接到 {{site.data.keyword.cloud_notm}}。您可以为各种工作负载（例如，认知、AI 和机器学习计算）创建最佳云。

下面是 VPC 支持混合云工作负载、云容错工作负载和云本机工作负载的更多方法：

* 具有水平自动缩放功能的负载均衡
* 监视和运行状况检查
* 支持 GPU
* 供应 VSI（具有亲缘关系和反亲缘关系组）

## 了解更多信息

* [**VPC 术语**](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-glossary)
* [**VPC 安全性**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-security-in-your-ibm-cloud-vpc#security-in-your-ibm-cloud-vpc)
* [**可用的 VPC 区域和子网**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)
* [**在 VPC 中创建和管理网络资源**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-network-resources-in-vpc)
* [**在 VPC 中创建和管理虚拟服务器实例**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-virtual-server-instances)
* [**在 VPC 中创建和管理块存储器**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-storage-in-vpc)
