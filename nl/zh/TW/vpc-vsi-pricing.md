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

# {{site.data.keyword.vsi_is_short}} 的定價 
{: #pricing-for-virtual-servers-for-vpc}
[comment]: # (鏈結的說明主題)


在最多有 62 個 vCPU 及 248 GB RAM 可容納任何工作負載需求的選取區中，提供 {{site.data.keyword.vsi_is_full}}。只會按小時向您收費，您的實例執行越久，即會套用折扣。對於實例的使用時間及暫停時間，虛擬伺服器使用時間會每秒計算。例如，如果您的實例執行 45 分鐘 32 秒，則會向您收取 45 分鐘 32 秒的費用。
{:shortdesc}

## 持續使用
{: #sustained-usage}

按小時收取實例的費用時，實例執行越久，費率就越低。隨著計費月份的進展，您會收到下列每小時折扣。

| 一個月所經歷的時間            | 計費折扣          | 
| ----------------------------- | ----------------- | 
| 0-20%                         | 一般零售費率      |                 
| 21-40%                        | 5%        |                  
| 41-60%                        | 10%       |                  
| 61-80%                        | 15%        |                  
| 81-100%                       | 20% |
{: caption="表 1. 分層折扣" caption-side="top"}  

這些折扣層級可讓您節省 10%，保持讓實例每月執行而非每小時執行。此折扣僅適用於基本每小時費率；它不適用於軟體、儲存空間、網路或其他費用。

<!-- As your workload demands change, you can always increase or decrease the size of your instance. If you resize to a larger instance size, the discounts reset and you pay the regular rate again. If you resize to a smaller instance size, the discounted rate does not reset. You continue to progress through the hourly discount tiers. -->

### 持續使用範例
{: #sustained-usage-example}

假設您購買具有 16 個 CPU 及 64 GB RAM 的「平衡」虛擬伺服器系列中的實例，基本價為每小時 $0.795。隨著月份的進展，您的費率降低如下：

| 一個月所經歷的時間            | 計費折扣          |  範例費率     |
| ----------------------------- | ----------------- | -------- |
| 0-20%                         | 一般零售費率 ($.795) | $116.07    |                
| 21-40%                        | 5%        |   $110.27   |                 
| 41-60%                        | 10%       |    $104.46  |            
| 61-80%                        | 15%        |    $98.66    |                
| 81-100%                       | 20% |       $92.86      |
{: caption="表 2. 分層折扣範例" caption-side="top"}  

具有此模型的總帳單（如果整個月持續執行）是 $522.32。相較於每小時費率，此折扣整個月可節省 10%。

## 基本實例價格
{: #base-instance-prices}

基本實例價格從每小時 $0.087 開始。當您建立虛擬伺服器時，系統會提示您選取虛擬伺服器系列，並選取設定檔配置。當您進行選擇時，表格中會顯示關聯的每小時費率。<!-- You can also use the Pricing Calculator to estimate your costs. --> 

## 包括的作業系統
{: #included-operating-systems}

下列是免費提供的作業系統：

* CentOS 7.latest
* Ubuntu 16.04 LTS、18.04（最小）
* Debian 8.latest、9.latest（最小）

有一些超值作業系統及其他附加程式可用。您將會看到「成本摘要」中反映的定價。

## 暫停計費
{: #suspend-billing}

關閉實例電源時，不會增加特定運算資源的成本。停止實例時，會自動停止計費。暫停計費特性可協助您減少成本，並防止在您再次需要實例的資源時重建實例。

如果您要擴增及縮減基礎架構以回應工作負載需求，可以使用暫停計費特性，作為更快速建立及刪除實例的替代方案。
{:tip}

### 計費詳細資料
{: #billing-details}

請務必瞭解關閉虛擬伺服器實例電源時成本停止增加的項目及成本持續保存的項目。

只有在您透過 IBM Cloud 主控台、CLI 或 API 關閉虛擬伺服器實例的電源時，才會暫停計費。如果您直接透過 OS 關閉虛擬伺服器實例的電源，並不會暫停該實例的計費。
{:note}

檢閱下表，以取得暫停計費如何影響各種資源費用的詳細資料。

| 資源                          | 停止計費          | 計費持續保存     |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| 頻寬升級                      |          X        |                  |
| 作業系統授權                  |          X        |                  |
| 浮動 IP、負載平衡器或其他連接的網路供應項目                          |                   |         X        |
| 儲存空間                      |                   |         X        |
{: caption="表 1. 資源計費詳細資料" caption-side="top"}   

對於虛擬伺服器實例的使用時間及暫停時間，每秒都會計算使用時間。即使您從未透過關閉實例的電源來起始暫停計費特性，還是會在實例生命週期內每秒計算費用。
{:note}

#### 暫停計費及持續使用折扣
{: #suspend-billing-and-sustained-usage-discounts}

若為持續使用折扣，已暫停的映像檔會挑選在折扣層級停止的位置。換言之，使用折扣只適用於使用映像檔的時間，而不適用於暫停映像檔的時間。

#### 使用費用下限
{: #minimum-usage-charge}

虛擬伺服器實例每個月有使用費用下限。如果計費週期的用量大於 25%，則會向您收取實際使用的費用。如果用量小於它在計費週期中存在時間的 25%，則會套用費用下限的 25%。 

例如，假設您的實例在整個計費週期（720 個小時）都在執行中。在該時間內，實例暫停執行 577 小時，並執行 143 小時。實例會以 180 小時計費（該計費期間的可用時數下限）。  

不過，假設您的實例在總共持續 400 小時的計費週期內建立及停止。在該時間內，實例暫停執行 120 小時，並執行 280 小時。將針對實例收取 280 小時用量的費用。

#### 計費發票
{: #billing-invoice}

當您暫停虛擬伺服器的計費時，將會在計費發票中看到一些變更。相關費用現在會顯示為以用量為基礎的詳細資料。例如，您可能會看到下列反映可用小時、已使用小時及已收費總時數的新增項目：

```
Computing instance usage...
RAM usage...
Operating system usage...
```
{:screen}

### 資源詳細資料
{: #resource-details}

#### 儲存空間
{: #suspend-billing-storage}

當您暫停虛擬伺服器實例的計費時，會持續保存關聯的儲存空間，但在關閉虛擬伺服器實例的電源時，您無法存取資料。當您繼續實例的計費時，即可接著存取您的資料，也會再次開始正常的計費。

#### IP 位址
{: #suspend-billing-ip-addresses}

暫停實例時，所有網路配置及 IP（來自子網路範圍的專用 IP）都會保持不變。

您可以在實例詳細資料頁面上檢視您的裝置是否已停止。若要查看狀態何時變更，請按一下導覽窗格中的**活動**。 

#### 限制
{: #suspend-billing-limitations}

暫停的虛擬伺服器會繼續列入您的整個帳戶裝置配額。
