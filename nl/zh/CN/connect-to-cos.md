---
copyright:
  years: 2018-2019
lastupdated: "2019-06-11"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, datacenter

subcollection: vpc-on-classic

---
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# 从 VPC 连接到 IBM Cloud Object Storage
{: #connecting-to-ibm-cloud-object-storage-from-a-vpc}

本文档说明如何从 Virtual Private Cloud 连接到 {{site.data.keyword.cloud}} Object Storage。端点信息特定适用于经典基础架构环境中的 {{site.data.keyword.cloud_notm}} Virtual Private Cloud。
{: shortdesc}


## 什么是 IBM Cloud Object Storage (COS)？
{: #what-is-ibm-cloud-object-storage-cos}

{{site.data.keyword.cloud}} Object Storage (COS) 是一个用于存储非结构化数据的 Web 级平台。它提供了可靠性、安全性、可用性和灾难恢复功能，无需进行复制。

存储在 {{site.data.keyword.cloud_notm}} Object Storage 中的信息会进行加密并分散在多个地理位置。可通过实现 S3 API 对其进行访问。此服务利用的是 IBM Cloud Object Storage System 提供的分布式存储技术。

IBM Cloud Object Storage 提供有三种类型的弹性配置：**跨区域**、**区域**和**单数据中心**。 
* **跨区域**服务提供的耐久性和可用性都要高于使用单个区域，但代价是等待时间略长。此服务如今在美国、欧洲和亚太地区可用。 
* **区域**服务的权衡则相反。它在单个区域内的多个可用性专区之间分配对象。此服务在美国区域、欧洲区域和亚太地区可用。如果给定区域或可用性专区不可用，对象存储也能不受影响继续运行。
**单数据中心**服务跨同一物理位置中的多个机器分配对象。请定期查看此文档以获取可用区域。

### 用于 VPC 的 COS 直接端点
{: #cos-direct-endpoints-for-use-with-vpc}

要发送 REST API 请求，必须设置目标端点或 URL，并且每个 COS 存储位置必须具有自己的一组 URL。直接端点为服务器提供与 {{site.data.keyword.cloud_notm}} 服务的高速直接连接，不会发生额外的带宽成本。通过 COS 直接端点，VPC 可以连接到位于世界任何位置的 COS 存储区。 

从 VPC 到此页面上列出的所有 COS 端点的流量均免费。
{: note}

* VPC 客户机还有权通过公共端点访问 COS 存储区。
* VPC 客户机只有权通过公共端点（而不是直接端点）访问 [COS 配置 API](https://{DomainName}/apidocs/cos/cos-configuration)。COS 配置 API 可用于在存储区配置一些 COS 功能，并可用于查看存储区元数据。
* 启用防火墙后，VPC 客户机无权访问 COS 存储区。

## 如何从 VPC 连接到 IBM Cloud Object Storage (COS)
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. 通过[目录 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/catalog/services/cloud-object-storage){: new_window} 供应 COS。
2. 在以下部分中列出的其中一个区域中创建 COS 存储区。
3. 使用端点与 COS 存储区进行通信。

### 区域端点
{: #regional-endpoints}

在区域端点上创建的存储区在三个数据中心分配数据，这三个数据中心分布在大城市区域中。如果其中有任何一个数据中心发生中断，或者甚至发生破坏，都不会影响可用性。

|**区域**|**端点**|
|------------|-------------------------------|
|美国南部|`s3.direct.us-south.cloud-object-storage.appdomain.cloud`|
|美国东部|`s3.direct.us-east.cloud-object-storage.appdomain.cloud`|
|欧洲英国|`s3.direct.eu-gb.cloud-object-storage.appdomain.cloud`|
|欧洲德国|`s3.direct.eu-de.cloud-object-storage.appdomain.cloud`|
|亚太地区澳大利亚|`s3.direct.au-syd.cloud-object-storage.appdomain.cloud`
|亚太地区日本|`s3.direct.jp-tok.cloud-object-storage.appdomain.cloud`|


### 跨区域端点
{: #cross-region-endpoints}

在跨区域端点创建的存储区会在三个区域中分配数据。如果其中有任何一个区域发生中断，或者甚至发生破坏，都不会影响可用性。请求会使用边界网关协议 (BGP) 路由来路由到距离最近区域的数据中心。

万一发生中断，请求会自动重新路由到活动区域。对于希望编写自己的故障转移逻辑的高级用户，可以通过向特定区域的端点发送请求并绕过 BGP 路由来实现。

|**美国跨区域**|**端点**|
|------------|-------------------------------|
|美国跨区域|`s3.direct.us.cloud-object-storage.appdomain.cloud`|
|达拉斯访问点|`s3.direct.dal.us.cloud-object-storage.appdomain.cloud`
|
|圣何塞访问点|`s3.direct.sjc.us.cloud-object-storage.appdomain.cloud`
|

|**欧洲跨区域**|**端点**|
|------------|-------------------------------|
|欧洲跨区域|`s3.direct.eu.cloud-object-storage.appdomain.cloud`|
|阿姆斯特丹访问点|`s3.direct.ams.eu.cloud-object-storage.appdomain.cloud`|
|法兰克福访问点|`s3.direct.fra.eu.cloud-object-storage.appdomain.cloud`|
|米兰访问点|`s3.direct.mil.eu.cloud-object-storage.appdomain.cloud`|

|**亚太地区跨区域**|**端点**|
|------------|-------------------------------|
|亚太地区跨区域|`s3.direct.ap.cloud-object-storage.appdomain.cloud`|
|东京访问点|`s3.direct.tok.ap.cloud-object-storage.appdomain.cloud`|
|首尔访问点|`s3.direct.seo.ap.cloud-object-storage.appdomain.cloud`|
|中国香港特别行政区访问点|`s3.direct.hkg.ap.cloud-object-storage.appdomain.cloud`|


 ### 单数据中心端点
 {: #single-datacenter-endpoints}

单数据中心不对 IBM Cloud 服务（例如，IAM 或 Key Protect）进行主机托管，并且在发生站点中断或破坏时，不会提供任何弹性。

如果联网故障生成分区，从而导致数据中心无法访问用于访问 IAM 的核心 IBM Cloud 区域，那么会从高速缓存中读取认证和授权信息，但这些信息可能已陈旧。此操作可能会导致在长达 24 小时内不强制实施新的或变更的 IAM 策略。
{:important}

|**区域**|**端点**|
|------------|-------------------------------|
|荷兰阿姆斯特丹|`s3.direct.ams03.cloud-object-storage.appdomain.cloud`|
|印度金奈|`s3.direct.che01.cloud-object-storage.appdomain.cloud`|
|中国香港特别行政区|`s3.direct.hkg02.cloud-object-storage.appdomain.cloud`|
|澳大利亚墨尔本|`s3.direct.mel01.cloud-object-storage.appdomain.cloud`|
|墨西哥墨西哥城|`s3.direct.mex01.cloud-object-storage.appdomain.cloud`|
|意大利米兰|`s3.direct.mil01.cloud-object-storage.appdomain.cloud`|
|加拿大蒙特利尔|`s3.direct.mon01.cloud-object-storage.appdomain.cloud`|
|挪威奥斯陆|`s3.direct.osl01.cloud-object-storage.appdomain.cloud`|
|美国圣何塞|`s3.direct.sjc04.cloud-object-storage.appdomain.cloud`|
|巴西圣保罗|`s3.direct.sao01.cloud-object-storage.appdomain.cloud`|
|韩国首尔|`s3.direct.seo01.cloud-object-storage.appdomain.cloud`|
|加拿大多伦多|`s3.direct.tor01.cloud-object-storage.appdomain.cloud`|
