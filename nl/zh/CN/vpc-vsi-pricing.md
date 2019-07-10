---



copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: pricing, billing, data, instance, VSI, VPC, vCPU, RAM, workload, discount, usage, minimum, invoice, delay, limitation, operating system, suspend

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# {{site.data.keyword.vsi_is_short}} 的定价 
{: #pricing-for-virtual-servers-for-vpc}
[comment]: # (链接的帮助主题)


{{site.data.keyword.vsi_is_full}} 在精选区域中提供最多 62 个 vCPU 和 248 GB RAM，以适应任何工作负载需求。仅按每小时费率计费，随着实例运行时间增加，将应用相应折扣。对于实例的使用时间和暂挂时间，会按秒计算虚拟服务器使用时间。例如，如果实例的运行时间是 45 分钟 32 秒，那么会按 45 分钟 32 秒计费。
{:shortdesc}

## 持续使用量
{: #sustained-usage}

虽然实例是按每小时费率收费的，但实例运行时间越长，费率越低。随着计费月份的时间流逝，您会收到以下每小时折扣。

|一个月中的使用时间|计费折扣| 
| ----------------------------- | ----------------- | 
| 0-20% | 常规零售费率 |                 
| 21-40% | 5% |                  
| 41-60% | 10% |                  
| 61-80% | 15% |                  
| 81-100% | 20% |
{: caption="表 1. 分层折扣" caption-side="top"}  

相对于实例按小时运行，实例保持按月运行时，这些折扣层可为您节省 10% 的费用。此折扣仅适用于基本每小时费率；不适用于软件、存储器、网络或其他费用。

<!-- As your workload demands change, you can always increase or decrease the size of your instance. If you resize to a larger instance size, the discounts reset and you pay the regular rate again. If you resize to a smaller instance size, the discounted rate does not reset. You continue to progress through the hourly discount tiers. -->

### 持续使用量示例
{: #sustained-usage-example}

假设您从均衡虚拟服务器系列购买了一个实例，其中包括 16 个 CPU 和 64 GB RAM，基本价格为 0.795 美元/小时。随着该月时间流逝，您的费率会下降，如下所示：

|一个月中的使用时间|计费折扣|示例费率|
| ----------------------------- | ----------------- | -------- |
| 0-20% | 常规零售费率（0.795 美元）| 116.07 美元 |                
| 21-40% | 5% | 110.27 美元 |                 
| 41-60% | 10% | 104.46 美元 |            
| 61-80% | 15% | 98.66 美元 |                
| 81-100% | 20% | 92.86 美元 |
{: caption="表 2. 分层折扣示例" caption-side="top"}  

此模型的总帐单（如果整个月一直保持运行）为 522.32 美元。相对于每小时费率，此折扣每月总体可节省 10% 的费用。

## 基本实例价格
{: #base-instance-prices}

基本实例价格的起步价是 0.087 美元/小时。创建虚拟服务器时，系统将提示您选择虚拟服务器系列，然后选择概要文件配置。进行选择时，关联的每小时费率会显示在表中。<!-- You can also use the Pricing Calculator to estimate your costs. --> 

## 随附的操作系统
{: #included-operating-systems}

免费随附以下操作系统：

* CentOS 7.latest
* Ubuntu 16.04 LTS 和 18.04（最低）
* Debian 8.latest 和 9.latest（最低）

有高端操作系统和其他附加组件可用。定价将反映在“成本摘要”中。

## 暂挂计费
{: #suspend-billing}

关闭实例的电源后，特定计算资源即不会再增加成本。停止实例时，计费会自动停止。暂挂计费功能可帮助您降低成本，并且您再次需要实例的资源时，无需重新创建该实例。

在要根据工作负载需求扩展和缩减基础架构的情况下，可以使用暂挂计费功能来替代创建和删除实例，这种方法速度更快。
{:tip}

### 计费详细信息
{: #billing-details}

请务必了解在虚拟服务器实例电源关闭时，哪些成本停止增加，哪些成本持续存在。

仅当通过 IBM Cloud 控制台、CLI 或 API 来关闭虚拟服务器实例的电源时，才会暂挂计费。如果是直接通过操作系统来关闭虚拟服务器实例的电源，那么不会暂挂对该实例的计费。
{:note}

请查看下表，以了解有关暂挂计费如何影响各种资源费用的详细信息。

|资源|计费已停止|计费持续存在|
| ----------------------------- | ----------------- | ---------------- |
|vCPU|X|                  |
|RAM|X|                  |
|带宽升级|X|                  |
|操作系统许可证|X|                  |
|浮动 IP、负载均衡器或其他连接的联网产品|                   |X|
|存储器|                   |X|
{: caption="表 1. 资源计费详细信息" caption-side="top"}   

对于虚拟服务器实例的使用时间和暂挂时间，会按秒计算使用时间。即使您关闭了实例电源，从未启动暂挂计费功能，也会根据实例的生命周期按秒来计费。
{:note}

#### 暂挂计费和持续使用量折扣
{: #suspend-billing-and-sustained-usage-discounts}

对于持续使用量折扣，暂挂的映像会在上次离开时的折扣层继续。换言之，使用量折扣仅应用于映像的使用时间，而不会应用于映像暂挂时间。

#### 最低使用量费用
{: #minimum-usage-charge}

虚拟服务器实例有每月最低使用量费用。如果在计费周期中使用时间超过 25%，那么会根据实际使用时间计费。如果使用时间低于计费周期的 25%，那么将按 25% 应用最低费用。 

例如，假设您有一个实例在整个计费周期（720 小时）内运行。在该时间内，实例的暂挂时间为 577 小时，运行时间为 143 小时。对于该实例，将按时间 180 小时（该计费周期中的最低可用小时数）收费。  

但是，假设您有一个实例，您在持续总计 400 小时的计费周期内创建和停止了该实例。在该时间内，实例的暂挂时间为 120 小时，运行时间为 280 小时。对于该实例，将按其 280 小时使用时间收费。

#### 计费发票
{: #billing-invoice}

暂挂虚拟服务器上的计费时，您将看到计费发票中的一些变化。相关费用现在显示为基于使用量的详细信息。例如，您可能看到反映可用小时数、已用小时数和收费小时总数的以下新增项：

```
Computing instance usage...
RAM usage...
Operating system usage...
```
{:screen}

### 资源详细信息
{: #resource-details}

#### 存储器
{: #suspend-billing-storage}

在虚拟服务器实例上暂挂计费时，关联的存储器会持续存在，但在虚拟服务器实例电源关闭期间，无法访问其中的数据。在实例上恢复计费后，才可以访问数据，并且会重新开始正常计费。

#### IP 地址
{: #suspend-billing-ip-addresses}

实例暂挂期间，所有网络配置和 IP（来自子网范围的专用 IP）都保持不变。

可以在实例详细信息页面上查看设备是否已停止。要查看状态何时发生更改，请单击导航窗格中的**活动**。 

#### 限制
{: #suspend-billing-limitations}

暂挂的虚拟服务器将继续计入帐户范围的设备配额。
