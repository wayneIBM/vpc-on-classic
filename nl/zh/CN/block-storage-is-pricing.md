---

copyright:
  years: 2019
lastupdated: "2019-05-21"

keywords: storage, capacity, billing, volume

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:new_window: target="_blank"}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Block Storage for VPC 的定价
{: #block-storage-pricing}

{{site.data.keyword.block_storage_is_short}} 的定价基于供应的容量或 IOPS，具体取决于您选择的存储概要文件。

|IOPS 层|发布的按月计费率^1 |
|------------|--------------|
|3 IOPS/GB|0.13 美元/GB|
|5 IOPS/GB|0.25 美元/GB|
|10 IOPS/GB|0.58 美元/GB|
|定制 IOPS|0.10 美元/GB + 0.07 美元/IOPS |

^1 发布的按月计费价格（[以小时为单位计算](#how-are-charges-calculated)）。

## 如何计算费用？
{: #how-are-charges-calculated}

{{site.data.keyword.block_storage_is_short}} 按小时计算，即根据块存储卷在帐户上存在的总小时数，直到卷被删除或计费周期结束（两者以先到者为准）。对于预定义 IOPS 层和定制 IOPS，按小时计费的计算方式不同。请参阅以下示例。

### 计算使用通用 IOPS 层的卷的计费
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

您供应了 1000 GB 卷，该卷使用通用 3 IOPS/GB 层，在使用该卷 72 小时后，将其删除。该卷的总价格按小时计费，如下所示：

((1000 GB x 0.13 美元/GB)/730 小时) x 72 小时 = $12.82

### 计算使用定制 IOPS 层的卷的计费
{: #billing-calculation-for-a-volume-with-custom-IOPS}

您供应了 1000 GB 卷，该卷使用 2500 IOPS，在使用该卷 72 小时后，将其删除。该卷的总价格按小时计费，如下所示：

(((1000 x 0.10 美元/GB) + (2500 x 0.07 美元)) / 730 小时) x 72 小时 = $27.12
