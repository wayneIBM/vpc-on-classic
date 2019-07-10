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

# 從 VPC 連接至 IBM Cloud Object Storage
{: #connecting-to-ibm-cloud-object-storage-from-a-vpc}

本文件說明如何從 Virtual Private Cloud 連接至 {{site.data.keyword.cloud}} Object Storage。端點資訊專門適用於標準基礎架構環境中的 {{site.data.keyword.cloud_notm}} Virtual Private Cloud。
{: shortdesc}


## 何謂 IBM Cloud Object Storage (COS)？
{: #what-is-ibm-cloud-object-storage-cos}

{{site.data.keyword.cloud}} Object Storage (COS) 是一個用於儲存非結構化資料的 Web 規模平台。它提供了可靠性、安全、可用性和災難回復功能，無需進行抄寫。

儲存在 {{site.data.keyword.cloud_notm}} Object Storage 中的資訊會進行加密並分散在多個地理位置。可透過 S3 API 的實作來存取它。本服務利用 IBM Cloud Object Storage System 所提供的分散式儲存技術。

IBM Cloud Object Storage 提供有三種類型的配置以進行備援：**跨地區**、**地區性**和**單一資料中心**。 
* **跨地區**服務提供的延續性和可用性都要高於使用單一地區，但代價是延遲略長。此服務如今在美國、歐洲和亞太地區可用。 
* **地區性**服務的取捨則相反。它會將物件分散在單一地區內的多個可用性區域。此服務在美國、歐洲和亞太地區可用。如果給定地區或區域無法使用，物件儲存庫也能不受影響繼續運行。
**單一資料中心**服務會將物件分散在相同實體位置內的多台機器。請定期查看此文件以取得可用地區。

### 用於 VPC 的 COS 直接端點
{: #cos-direct-endpoints-for-use-with-vpc}

若要傳送 REST API 要求，必須設定目標端點或 URL，並且每個 COS 儲存位置必須具有自己的一組 URL。直接端點為伺服器提供與 {{site.data.keyword.cloud_notm}} 服務的高速直接連線，不會增加頻寬成本。透過 COS 直接端點，VPC 可以連接至位於世界任何位置的 COS 儲存區。 

從 VPC 到此頁面上列出的所有 COS 端點的資料流量均免費。
{: note}

* VPC 用戶端還有權透過公用端點存取 COS 儲存區。
* VPC 用戶端只有權透過公用端點（而不是直接端點）存取 [COS 配置 API](https://{DomainName}/apidocs/cos/cos-configuration)。COS 配置 API 可用於在儲存區配置一些 COS 特性，並可用於檢視儲存區 meta 資料。
* 已啟用防火牆後，VPC 用戶端無權存取 COS 儲存區。

## 如何從 VPC 連接至 IBM Cloud Object Storage (COS)
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. 從[型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/catalog/services/cloud-object-storage){: new_window} 佈建 COS。
2. 在下列區段中列出的其中一個地區中建立 COS 儲存區。
3. 使用端點與 COS 儲存區進行通訊。

### 地區端點
{: #regional-endpoints}

在地區性端點上建立的儲存區會在三個資料中心之間分散資料，這三個資料中心分佈在一個大城市區域中。如果其中有任何一個資料中心發生中斷，或者甚至毀損，都不會影響可用性。

| **地區** | **端點** |
|------------|-------------------------------|
|美國南部| `s3.direct.us-south.cloud-object-storage.appdomain.cloud`|
|美國東部| `s3.direct.us-east.cloud-object-storage.appdomain.cloud`|
|歐盟英國| `s3.direct.eu-gb.cloud-object-storage.appdomain.cloud`|
|歐盟德國| `s3.direct.eu-de.cloud-object-storage.appdomain.cloud`|
|亞太澳洲| `s3.direct.au-syd.cloud-object-storage.appdomain.cloud`
|亞太日本| `s3.direct.jp-tok.cloud-object-storage.appdomain.cloud` |


### 跨地區端點
{: #cross-region-endpoints}

在跨地區端點建立的儲存區會在三個地區之間分散資料。如果其中有任何一個地區發生中斷，或者甚至毀損，都不會影響可用性。要求會使用邊界閘道通訊協定 (BGP) 遞送來遞送至距離最近地區的資料中心。

萬一發生中斷，要求會自動重新遞送到作用中地區。對於希望編寫自己的失效接手邏輯的進階使用者，可以透過向特定地區的端點傳送要求並略過 BGP 遞送來這樣做。

|**美國跨地區**| **端點** |
|------------|-------------------------------|
|美國跨地區| `s3.direct.us.cloud-object-storage.appdomain.cloud` |
| 達拉斯存取點 | `s3.direct.dal.us.cloud-object-storage.appdomain.cloud`
|
| 聖荷西存取點 | `s3.direct.sjc.us.cloud-object-storage.appdomain.cloud`
|

|**歐洲跨地區**| **端點** |
|------------|-------------------------------|
|歐盟跨地區|`s3.direct.eu.cloud-object-storage.appdomain.cloud`|
|阿姆斯特丹存取點|`s3.direct.ams.eu.cloud-object-storage.appdomain.cloud`|
|法蘭克福存取點|`s3.direct.fra.eu.cloud-object-storage.appdomain.cloud`|
|米蘭存取點|`s3.direct.mil.eu.cloud-object-storage.appdomain.cloud`|

|**亞太地區跨地區**| **端點** |
|------------|-------------------------------|
|亞太跨地區|`s3.direct.ap.cloud-object-storage.appdomain.cloud`|
|東京存取點|`s3.direct.tok.ap.cloud-object-storage.appdomain.cloud`|
|首爾存取點|`s3.direct.seo.ap.cloud-object-storage.appdomain.cloud`|
|中國香港特別行政區存取點|`s3.direct.hkg.ap.cloud-object-storage.appdomain.cloud`|


 ### 單一資料中心端點
 {: #single-datacenter-endpoints}

單一資料中心不對 IBM Cloud 服務（例如，IAM 或 Key Protect）進行主機代管，並且在發生網站中斷或毀損時，不會提供任何備援。

如果網路連線功能故障產生分割區，而導致資料中心無法存取用於存取 IAM 的核心 IBM Cloud 地區，則會從快取中讀取鑑別和授權資訊，但這些資訊可能已過時。此動作可能會導致在最長 24 小時的時間內不強制實施新的或變更的 IAM 原則。
{:important}

| **地區** | **端點** |
|------------|-------------------------------|
|荷蘭阿姆斯特丹|`s3.direct.ams03.cloud-object-storage.appdomain.cloud`|
|印度清奈|`s3.direct.che01.cloud-object-storage.appdomain.cloud`|
|香港|`s3.direct.hkg02.cloud-object-storage.appdomain.cloud`|
|澳大利亞墨爾本|`s3.direct.mel01.cloud-object-storage.appdomain.cloud`|
|墨西哥墨西哥城|`s3.direct.mex01.cloud-object-storage.appdomain.cloud`|
|義大利米蘭|`s3.direct.mil01.cloud-object-storage.appdomain.cloud`|
|加拿大蒙特利爾| `s3.direct.mon01.cloud-object-storage.appdomain.cloud` |
|挪威奧斯陸|`s3.direct.osl01.cloud-object-storage.appdomain.cloud`|
|美國聖荷西|`s3.direct.sjc04.cloud-object-storage.appdomain.cloud`|
|巴西聖保羅|`s3.direct.sao01.cloud-object-storage.appdomain.cloud`|
|韓國首爾|`s3.direct.seo01.cloud-object-storage.appdomain.cloud`|
| 加拿大多倫多 | `s3.direct.tor01.cloud-object-storage.appdomain.cloud` |
