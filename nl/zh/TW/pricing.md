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


# VPC 的定價
{: #pricing-for-vpc}

下表彙總使用 {{site.data.keyword.cloud}} Virtual Private Cloud 進行網際網路資料傳送的定價。請記住，VPC 與其他標準 IBM Cloud 服務之間的資料流量是免費的。此外，對於 PayGo 服務，服務層也會連結至您的帳戶，而不是連結至任何特定的 VPC 實例。「金額」會以美元為單位顯示。

個別定價適用於 IBM Cloud VPC 內所使用的[虛擬伺服器實例](/docs/vpc-on-classic?topic=vpc-on-classic-pricing-for-virtual-servers-for-vpc)和[區塊儲存空間](/docs/vpc-on-classic?topic=vpc-on-classic-block-storage-pricing)。

## 網際網路資料傳送的免費額度
{: #free-allowances-for-internet-data-transfer}

| 資料傳送 | 所有 IBM Cloud VPC 客戶的成本 |
|---------------|------------------|
| 區域內 | 免費 |
| 相同地區的各區域之間 | 免費 |
| 使用公用閘道 | 免費（只有針對 PGW 所使用的浮動 IP 才需要收費）|

## 浮動 IP 定價
{: #floating-ip-pricing}

浮動 IP 按費率 1 美元/月收費，從保留之日起開始計算。即使浮動 IP 未與 VSI 相關聯或未使用中，還是會收取費用。即使浮動 IP 僅保留幾天，還是會收取每個月 $1 的費用。

## 網際網路資料傳送的基本 PayGo 定價
{: #basic-paygo-pricing-for-internet-transfer}

| 資料傳送 | 資料量 | PayGo 定價 |
|-----------|-----------|------------------|
| 輸出至網際網路 | 0 到 5 GB | 免費 |
|  | 6 到 10,000 GB | 每 GB $0.087 |
|  | 10,001 到 50,000 GB | 每 GB $0.083 |
|  | 50,001 到 150,000 GB | 每 GB $0.07 |
|  | 150,001 GB 及以上 | 每 GB $0.05 |


當您建立新的 VPC 時，起始計費費用最多可能需要一小時才會出現在「主控台使用者介面」或 API 中。
{: tip}

如果您有公用閘道或「浮動 IP」，則可能仍然會看到一些最少輸出費用，即使您在該期間未送出任何輸出資料流量。這些費用用於 ARP 資料流量，這是操作帳戶的必要項目。
{: important}

## LBaaS for VPC 定價
{: #lb-for-vpc-pricing}

Load Balancer for VPC 定價基於下列度量，按月計算：
* 服務使用小時數
* 處理的資料


| 地區   |LB 服務用量（每小時）|每 GB 處理的 LB 資料|
|------------|--------------------------|-------------------------|
| 達拉斯 | $0.025 | $0.008 |
| 法蘭克福  | $0.027 | $0.009 |
| 東京  | $0.028 | $0.009 |

價格因多區域地區而有所不同。
{: note}

下列圖表顯示了每個月使用 4500 GB 進行公用負載平衡的客戶範例，其中單位為美金，位置為達拉斯地區。

|項目        |用量| 速率 |成本|
|---------|--------|---------|---------|          
|LB 服務用量| 720 小時 |0.025 美元/小時|每個月 18 美元|
|處理的資料|4500 GB|0.008 美元/GB|每個月 36 美元|

**表格 1：按月成本範例。此情境的總費用為每個月 54 美元。**


## VPN for VPC 定價
{: #vpn-for-vpc-pricing}

| 地區   |連線（同層級）- 每小時|實例（閘道）- 每小時|
|------------|--------------------------|-------------------------|
| 達拉斯 |0.04 美元|0.045 美元|
| 法蘭克福  |0.044 美元|0.0495 美元|
| 東京  |0.0452 美元|0.0585 美元|

對於因使用 VPNaaS 而向網際網路傳輸的資料，將按出埠到網際網路的常規資料傳送在 VPC 層次計費並收費。
{: note}
