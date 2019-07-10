---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-12"

keywords: limitations, bugs, known, known issues, Beta, services, capabilities, use cases

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# 已知限制
{: #known-limitations}

本文档包含不支持的功能和 API 的摘要、尚不支持的功能和用例、当前发行版中的已知问题以及仅作为 Beta 服务提供的功能的列表。已知限制可能会在我们向 {{site.data.keyword.vpc_full}} (VPC) 添加功能时发生变化，因此欢迎不时查看本文档。

## 不支持的功能摘要
{: #summary-of-features-not-supported}

* 仅通过[**经典访问**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)支持对 VPC 的 Direct Link 访问。
* 私有服务端点的子集可用于 VPC，但并非所有端点都可用。请参阅[可用于 IBM Cloud VPC 的服务端点](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)，以获取可用的服务。
* 不支持保存和复原映像。
* {{site.data.keyword.cloud_notm}} VPC 是区域性的，因此一个区域中的 VPC 无法连接到其他区域中的 VPC，除非这些 VPC 启用了“经典访问”，或者通过 VPN 连接建立了连接。请参阅[**经典访问**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)。

## 尚不支持的功能和用例
{: #features-and-use-cases-not-yet-supported}

此部分列出了不支持的功能和用例的更多详细信息，这些信息根据其 {{site.data.keyword.cloud_notm}} 服务分类。

### Virtual Private Cloud
{: vpc-unsupported-features-and-use-cases}

* {{site.data.keyword.vpc_short}} 不支持多点广播或广播域。
* {{site.data.keyword.vpc_short}} 不提供端到端加密。
* 一个 VPC 不能与其他 VPC 建立对等连接。
* VPC 不支持云服务端点。请参阅[可用于 IBM Cloud VPC 的服务端点](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)，以获取可用的服务。

### 计算
{: VSI-unsupported-features-and-use-cases}

* 现有 {{site.data.keyword.cloud_notm}} 虚拟服务器实例 (VSI) 或裸机服务器无法迁移到 VPC 中。
* 裸机服务器无法在 VPC 中进行供应。

### 网络
{: network-unsupported-features-and-use-cases}

* 定制路径无法添加到 VPC。缺省情况下，一个 VPC 中的所有子网都可以相互通信。
* 一个子网不能位于多个专区上。
* 子网不能从一个专区移动到另一个专区。
* 子网创建后即无法调整大小。
* {{site.data.keyword.cloud_notm}} 经典资源不能位于 VPC 中的同一子网中。经典资源与帐户中一个 VPC 之间的连接可以使用[**经典访问**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)来建立。
* 不支持 IPv6。
* 不能将自己的公共 IP 用作浮动 IP。浮动 IP 池由 IBM 提供。
* 浮动 IP 绑定到专区。例如，专区 1 中的池保留的浮动 IP 不能分配给同一 VPC 中专区 2 中的 VSI。
* 不能在 VSI 之间移动专用 IP 地址。
* 不能在 VSI 之间移动网络接口。
* 不能指定 VSI 的特定 IP 地址。只能指定子网的 IP 范围。将 VSI 连接到子网后，系统会从该子网中分配专用 IP。
* {{site.data.keyword.vpc_short}} 随附其自己的网络即服务工具，例如 LBaaS、ACL 和安全组。您不能使用自己的网络设备（例如，Vyatta 网关或其他负载均衡器）来控制 VPC 中的任何资源。与此类似，您也无法在 {{site.data.keyword.cloud_notm}} 经典基础架构中使用自己的负载均衡器或安全组服务来控制 {{site.data.keyword.vpc_short}} 中的资源。
* 公共网关不允许从因特网发起到 IBM Cloud VPC 中的 VSI 的流量。对于该用途，必须使用浮动 IP。
* ACL 是无状态的，因此必须在 ACL 规则中显式允许返回流量。

## 此发行版中的已知问题
{: #known-issues-in-this-release}

在以下所有情况下，{{site.data.keyword.block_storage_is_short}} 可能无法准确验证卷名的唯一性：

* 供应请求是并发的
* 有名称相同的卷
* 卷位于同一区域中

## 作为 Beta 服务提供的功能

与设备安全地建立对等连接作为 Beta 服务提供。有关与多种类型的设备建立对等连接的文档，请参阅：

* [远程 Vyatta 同级](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-vyatta-peer)
* [远程 StrongSwan 同级](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-strongswan-peer)
* [远程 FortiGate 同级](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-fortigate-peer)
* [远程 Juniper vSRX 同级](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-juniper-vsrx-peer)
* [远程 Cisco ASAv 同级](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-cisco-asav-peer)
