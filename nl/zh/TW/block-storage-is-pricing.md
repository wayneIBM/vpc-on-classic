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

# Block Storage for VPC 的定價
{: #block-storage-pricing}

{{site.data.keyword.block_storage_is_short}} 的定價是基於已佈建的容量或 IOPS，視您選擇的儲存空間設定檔而定。

|IOPS 層級|發佈的按月計費率^1 |
|------------|--------------|
|3 IOPS/GB|0.13 美元/GB|
|5 IOPS/GB|0.25 美元/GB|
|10 IOPS/GB |0.58 美元/GB|
|自訂 IOPS|0.10 美元/GB + 0.07 美元/IOPS |

^1 發佈的按月計費價格（[按小時計算](#how-are-charges-calculated)）。

## 如何計算費用？
{: #how-are-charges-calculated}

{{site.data.keyword.block_storage_is_short}} 根據區塊儲存空間磁區在帳戶上存在的總小時數，按小時計算，直到磁區被刪除或計費週期結束（兩者以先發生者為準）。對於預先定義 IOPS 層級和自訂 IOPS，按小時計費的計算方式不同。請參閱下列範例。

### 使用通用 IOPS 層級的磁區的計費計算
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

您佈建了 1000 GB 磁區，該磁區使用通用 3 IOPS/GB 層級，在使用該磁區 72 小時後，將其刪除。磁區的總價是按小時計費，如下所示：

((1000 GB x 0.13 美元/GB)/730 小時) x 72 小時 = 12.82 美元

### 使用自訂 IOPS 層的磁區的計費計算
{: #billing-calculation-for-a-volume-with-custom-IOPS}

您佈建了 1000 GB 磁區，該磁區使用 2500 IOPS，在使用該磁區 72 小時後，將其刪除。磁區的總價是按小時計費，如下所示：

(((1000 x 0.10 美元/GB) + (2500 x 0.07 美元)) / 730 小時) x 72 小時 = 27.12 美元
