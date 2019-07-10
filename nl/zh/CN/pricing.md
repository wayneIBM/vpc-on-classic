---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-29"

keywords: pricing, billing, data, instance, VSI, block, storage, paygo, transfer, floating, server, VPC, allowance, gateway, egress, minimal charges, ARP, traffic

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


# VPC 的定价
{: #pricing-for-vpc}

下表概述了使用 {{site.data.keyword.cloud}} Virtual Private Cloud 进行因特网数据传输的定价。请记住，VPC 与经典 IBM Cloud 服务之间的流量是免费的。此外，对于现收现付服务，服务层是绑定到您的帐户，而不是绑定到任何特定 VPC 实例。显示的金额以美元为单位。

单独定价适用于 IBM Cloud VPC 中使用的[虚拟服务器实例](/docs/vpc-on-classic?topic=vpc-on-classic-pricing-for-virtual-servers-for-vpc)和[块存储器](/docs/vpc-on-classic?topic=vpc-on-classic-block-storage-pricing)。

## 因特网数据传输的免费限额
{: #free-allowances-for-internet-data-transfer}

|数据传输|适用于所有 IBM Cloud VPC 客户的成本|
|---------------|------------------|
|在专区内|免费|
|在同一区域的专区之间|免费|
|使用公共网关|免费（仅针对 PGW 使用的浮动 IP 收费）|

## 浮动 IP 定价
{: #floating-ip-pricing}

浮动 IP 按费率 1 美元/月收费，从保留之日起开始计算。即使浮动 IP 未关联到 VSI 或未在使用中，也会收取费用。即使浮动 IP 只保留了几天，也会收取该月的费用，即 1 美元。

## 因特网数据传输的基本现收现付定价
{: #basic-paygo-pricing-for-internet-transfer}

|数据传输|数据量|现收现付定价|
|-----------|-----------|------------------|
|输出到因特网|0 到 5 GB|免费|
|  |6 到 10,000 GB|0.087 美元/GB|
|  |10,001 到 50,000 GB|0.083 美元/GB|
|  |50,001 到 150,000 GB|0.07 美元/GB|
|  |150,001 GB 及以上|0.05 美元/GB|


创建新的 VPC 时，初始计费费用可能需要一小时才会显示在控制台 UI 或 API 中。
{: tip}

如果您有公共网关或浮动 IP，那么即使在该时间内未发送任何输出流量，也仍然可能会看到一些最低输出费用。这些是针对 ARP 流量收取的费用，ARP 流量对于运行您的帐户是必需的。
{: important}

## LBaaS for VPC 定价
{: #lb-for-vpc-pricing}

Load Balancer for VPC 定价基于以下度量值，按月计算：
* 服务使用小时数
* 处理的数据量


|区域|LB 服务使用量（每小时）|处理的 LB 数据量（每千兆字节 (GB)）|
|------------|--------------------------|-------------------------|
|达拉斯|0.025 美元|0.008 美元|
|法兰克福|0.027 美元|0.009 美元|
|东京|0.028 美元|0.009 美元|

价格因多专区区域而有所不同。
{: note}

以下图表显示了每月使用 4500 GB 进行公共负载均衡的客户示例，其中单位为美元，位置为达拉斯区域。

|项目|使用量|费率|成本|
|---------|--------|---------|---------|          
|LB 服务使用量|720 小时|0.025 美元/小时|每月 18 美元|
|处理的数据量|4500 GB|0.008 美元/GB|每月 36 美元|

**表 1：每月成本示例。此场景的总费用为每月 54 美元。**


## VPN for VPC 定价
{: #vpn-for-vpc-pricing}

|区域|连接（同级）- 每小时|实例（网关）- 每小时|
|------------|--------------------------|-------------------------|
|达拉斯|0.04 美元|0.045 美元|
|法兰克福|0.044 美元|0.0495 美元|
|东京|0.0452 美元|0.0585 美元|

对于因使用 VPNaaS 而向因特网传输的数据，将按出站到因特网的常规数据传输在 VPC 级别计费并收费。
{: note}
