---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, storage, boot, block, volume, name, naming, best practices

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}

# 在 VPC 中创建和管理块存储器
{: #creating-and-managing-storage-in-vpc}

{{site.data.keyword.cloud}} Virtual Private Cloud 提供引导存储卷，并允许使用其他块存储卷，这些卷是针对虚拟服务器实例 (VSI) 通过系统管理程序安装的高性能数据存储器。VPC 基础架构可跨多个区域和专区快速缩放存储器，具有额外的性能和安全性。

## 特征摘要
{: #summary-of-characteristics}

供应 {{site.data.keyword.vsi_is_full}} 实例时，会创建一个 100 GB 的通用 IOPS (3 IOPS/GB) 块存储卷作为主引导卷，并连接到该 VSI。在供应期间，可以重命名引导卷并更改卷大小。
{:shortdesc}

* 每当在 VPC 网络中供应虚拟服务器实例时，都可以创建块存储卷。  
* 可以创建独立于 VSI 生命周期的新卷，并且以后将这些卷连接到 VSI。
* 可以自动创建连接到 VSI 的辅助数据卷。 
* 数据卷从 VSI 拆离后，会持久存储。因此，您以后可以将该卷连接到新实例。 
* 可以指定删除 VSI 时是否自动删除数据卷。  
* 删除引导卷的 VSI 时，会删除这些引导卷。
* 缺省情况下，引导卷和数据卷使用 IBM 管理的加密进行加密。 
* 在 VSI 供应期间或创建独立卷时，您可以使用自己的加密密钥对卷进行加密。


## 块存储卷
{: #block-storage-volumes}

有关使用块存储卷和 VPC 的更多完整信息，请参阅 [Block Storage for VPC 文档](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)。

有关 VPC 的块存储卷的概述信息，请参阅[关于 Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about)。 

要开始创建独立于 VSI 供应的卷，请参阅 [ {{site.data.keyword.block_storage_is_short}} 入门](/docs/infrastructure/block-storage-is?topicid=block-storage-is-block-storage-getting-started)。



## 创建和命名 VPC 块存储卷的最佳实践：
{: #best-practices-for-creating-and-naming-your-vpc-block-storage-volumes}

* 在供应卷之前确定存储需求。允许提供足够的容量和 IOPS 性能。
* 决定预定义的 IOPS 概要文件是否最符合您的容量和性能需求，或者是指定定制设置更好。
* 决定是否要在虚拟服务器实例供应期间创建辅助数据卷。这些卷会自动连接到实例。
* 决定是否要在删除 VSI 时自动删除连接到 VSI 的卷。
* 使用您自己的加密密钥加密卷时，请确保密钥有效，您有权使用该密钥，并且可以在当前区域中使用该密钥。
* 确保在供应时对所有卷命名，包括引导卷。
* 确保在连接时对所有卷连接命名。
* 每个卷在一个帐户的一个区域内必须具有不同的名称。

删除卷后，可以复用该卷的名称。但是，请注意，复用卷名可能会导致在计费时引起混淆。
{:note}
