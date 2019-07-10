---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: glossary, terminology, definition, access, floating, geography, image, region, zone, instance, VSI, LBaaS, VPN, VPC, NAT, profile, resource, security group, shares, storage, VNPaaS, volume, subnet, SSH, key, gateway, ACL

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

# VPC 词汇表
{: #vpc-glossary}

下面是通常使用的术语，也是特定于 {{site.data.keyword.cloud}} Virtual Private Cloud 的术语。有关更广泛、更多的术语，请参阅 [IBM Cloud 词汇表术语](/docs/overview/glossary?topic=overview-glossary)。

## 访问控制表 (Access Control List)
{: #access-control-list}

访问控制表 (ACL) 为[子网](#subnet)的入站和出站流量提供安全控制。ACL 是无状态的。每个 ACL 都由规则组成，这些规则基于_源 IP_、_源端口_、_目标 IP_、_目标端口_和_协议_。

在 IBM Cloud VPC 中，每个子网都创建有一个缺省 ACL，用于允许入站和出站流量，但客户可以创建定制 ACL。在任何时候，一个子网只有一个连接的 ACL，但一个 ACL 可以连接到多个子网。

有时也称为_网络 ACL_ (NACL)。

## 浮动 IP (Floating IP)
{: #floating-ip}

浮动 IP 方法用于通过从池中分配的_浮动 IP 地址_，为 VPC 资源（例如，实例、负载均衡器或 VPN 隧道）提供对因特网的入站和出站访问。

浮动 IP 地址是系统从预先存在的池中提供的公共 IP 地址。您不能使用自带公共 IP 地址。浮动 IP 地址可从因特网进行访问，并且可以通过 vNIC 关联到实例（例如，VSI、负载均衡器或 VPN 网关）。

可以保留 IBM 提供的可用浮动 IP 地址池中的浮动 IP 地址，也可以将其与同一虚拟私有云中的任何实例相关联或解除与任何实例的关联。浮动 IP 利用一对一 [NAT](#nat) 来允许服务器与公用因特网进行通信，还允许在云环境中与专用子网进行通信。

## 地理位置 (Geography)
{: #geography}

在 VPC 中，指定地理位置提供有关部署了 VPC 及其关联资源的区域和专区的信息。

## 映像 (Image)
{: #image}

启动虚拟服务器实例（即_实例_）所需的信息。通常，这是商业可用操作系统的快照映像，目的是用于引导。

## 隐式路由器 (Implicit Router)
{: #implicit-router}

在 VPC 中创建的所有子网之间的固有网络连接。

## 实例 (Instance)
{: #instance}

在 VPC 中运行的虚拟服务器（即 VSI）的短名称。

## LBaaS
{: #lbaas}

负载均衡器即服务 (LBaaS) 提供了在 VPC 中的实例之间均衡分配流量的能力。

## NAT
{: #nat}

网络地址转换 (NAT) 是 [Internet RFC 1631 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://tools.ietf.org/html/rfc1631){: new_window} 中描述的寻址方法，该方法可以使用一个 IP 地址与多个其他 IP 地址（例如，专用子网上的 IP 地址）进行通信，本质上是通过查找表进行通信。NAT 具有两种主要类型：一对一 NAT 和[多对一 NAT ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Network_address_translation){: new_window}。NAT 最初用于延长 IPv4 IP 地址的使用寿命，但现在，它在云环境中常用于创建与专用子网的通信。

## 概要文件 (Profile)
{: #profile}

概要文件是 vCPU 和 RAM 的常用组合，可以快速实例化以启动虚拟服务器实例 (VSI)。支持三个系列的概要文件：均衡、计算和内存。


## 公共网关 (Public Gateway)
{: #public-gateway}

公共网关 (PGW) 为子网（及连接到该子网的所有 VSI）启用仅出站访问权以用于连接到因特网。请注意，缺省情况下子网是专用的；但是，您可以选择创建 PGW，然后将子网连接到 PGW。在将子网连接到 PGW 后，该子网中的所有 VSI 都可以连接到因特网。

PGW 使用多对一 NAT，这意味着数千个使用专用地址的 VSI 将使用一个公共 IP 地址与公用因特网对话。PGW 不支持因特网启动与这些实例的连接。

## 区域 (Region)
{: #region}

部署 VPC 的地理区域。每个区域都包含多个专区，专区表示独立的故障区。IBM Cloud VPC 可跨其所分配区域内的多个专区。

## 资源 (Resource)
{: #resource}

作为 VPC 部署的一部分并且会产生计费费用的一种资产类型，例如 VSI、浮动 IP 或 VPC 本身。资源可以组合成_资源组_，以便在 VPC 中某些资源始终组合使用时，使记帐更容易。

## 安全组 (Security Group)
{: #security-group}

安全组充当虚拟防火墙，用于控制一个或多个服务器 (VSI) 的入站和出站流量。安全组是规则的集合，用于指定是否允许关联 VSI 的流量。客户启动 VSI 时，可以将一个或多个安全组与该 VSI 相关联。如果有正确的许可权，客户可以修改安全组规则。

## 共享 (Shares)
{: #shares}

一种在 VPC 中提供文件存储器的方法。

## 子网 (Subnet)
{: #subnet}

子网是一个 IP 地址范围，绑定到单个专区，不能跨多个专区或区域。子网可以跨 IBM Cloud VPC 中的一整个专区。

对于 IBM Cloud VPC 的用途，子网的重要特征是，子网既可以相互隔离，也能以常用方式互连。子网隔离可以通过网络访问控制表 (ACL) 来实现，ACL 充当防火墙，用于控制子网之间的数据包流。与此类似，安全组充当虚拟防火墙，用于控制进出各个虚拟服务器实例 (VSI) 的数据包流。

正是子网的这种隔离特性，允许您在公共云中创建专用空间。

## SSH 密钥 (SSH Key)
{: #ssh-key}

自动生成的加密访问凭证，用于控制对 VPC 中虚拟服务器实例的访问。

## 卷 (Volumes)
{: #volumes}

针对 VPC 中的虚拟服务器实例，通过系统管理程序安装的高性能数据存储。

## VPC
{: #vpc}

绑定到帐户的虚拟网络。它提供了对虚拟基础架构和网络流量分段的细颗粒度控制，还提供了安全性和动态缩放能力。

## VPN
{: #vpn}

虚拟专用网 (VPN) 允许在两个端点之间建立专用连接，即使在公用网络中传输数据时也能建立此连接。客户可以像连接到专用网络一样共享数据。通常，VPN 会与各种安全方法（例如，认证和加密）组合使用，以提供最高的数据安全性和隐私性。

## VPN 连接 (VPN Connection)
{: #vpn-connection}

两个端点之间的专用连接，此连接会保持专用并且可以加密，即使在公用网络中传输数据时也能建立此连接。

## VPNaaS
{: #vpnaas}

VPN 即服务 (VPNaaS) 允许通过 VPC 内部和外部的端点设置 VPN。

## 专区 (Zone)
{: #zone}

一种独立的故障区。专区是一种抽象概念，旨在帮助提高容错能力，缩短等待时间。专区可保证以下特性：

 * 由于每个专区都是独立的故障区，因此一个区域中的两个专区同时发生故障的可能性微乎其微。
 * 一个区域中各个专区之间的流量等待时间不到 2 毫秒。
