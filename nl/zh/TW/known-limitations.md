---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-12"

keywords: limitations, bugs, known, known issues, Beta, services, capabilities, use cases

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
{:DomainName: data-hd-keyref="DomainName"}

# 已知限制
{: #known-limitations}

本文件包含不支援的特性和 API 的摘要、尚不支援的特性和使用案例、現行版本中的已知問題以及僅作為測試版服務提供的特性的清單。已知限制可能會在我們對 {{site.data.keyword.vpc_full}} (VPC) 新增功能時發生變化，因此歡迎隨時查看本文件。

## 不受支援特性的摘要
{: #summary-of-features-not-supported}

* 只有透過[**標準存取**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)才支援對 VPC 的 Direct Link 存取。
* 專用服務端點的子集可用於 VPC，但並非所有端點都可用。如需可用的服務，請參閱 [IBM Cloud VPC 可用的服務端點](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)。
* 不支援儲存和還原映像檔。
* {{site.data.keyword.cloud_notm}} VPC 是地區性的，因此一個地區中的 VPC 無法連接至其他地區中的 VPC，除非這些 VPC 已啟用「標準存取」，或者藉由 VPN 連線建立了連線。請參閱[**標準存取**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)。

## 尚未支援的特性及使用案例
{: #features-and-use-cases-not-yet-supported}

此區段列出了不受支援的特性和使用案例的更多詳細資料，這些資訊根據其 {{site.data.keyword.cloud_notm}} 服務分類。

### Virtual Private Cloud
{: vpc-unsupported-features-and-use-cases}

* {{site.data.keyword.vpc_short}} 不支援多重播送或播送網域。
* {{site.data.keyword.vpc_short}} 不提供端對端加密。
* 一個 VPC 不能與其他 VPC 建立對等連接。
* VPC 不支援雲端服務端點。如需可用的服務，請參閱 [IBM Cloud VPC 可用的服務端點](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)。

### 運算
{: VSI-unsupported-features-and-use-cases}

* 現有 {{site.data.keyword.cloud_notm}} 虛擬伺服器實例 (VSI) 或 Bare Metal Server 伺服器無法移轉到 VPC 中。
* Bare Metal Server 伺服器無法在 VPC 中進行佈建。

### Network
{: network-unsupported-features-and-use-cases}

* 自訂路徑無法新增到 VPC。依預設，一個 VPC 中的所有子網路都可以彼此通訊。
* 子網路不能位於多個區域上。
* 子網路無法從某個區域移至另一個區域。
* 子網路在建立之後即無法調整大小。
* {{site.data.keyword.cloud_notm}} 標準資源不能位於 VPC 中的相同子網路中。標準資源與帳戶中一個 VPC 之間的連線功能可以使用[**標準存取**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)來建立。
* 不支援 IPv6。
* 您不能使用自己的公用 IP 作為「浮動 IP」。「浮動 IP」儲存區由 IBM 提供。
* 「浮動 IP」已連結至區域。例如，無法將「區域 1」中儲存區內所保留的「浮動 IP」指派給相同 VPC 中「區域 2」內的 VSI。
* 無法在 VSI 之間移動專用 IP 位址。
* 無法在 VSI 之間移動網路介面。
* 您無法指定 VSI 的特定 IP 位址。您只能指定子網路的 IP 範圍。將 VSI 連接至子網路之後，系統會指派來自該子網路的專用 IP。
* {{site.data.keyword.vpc_short}} 隨附其自己的網路即服務工具，例如 LBaaS、ACL 和安全群組。您不能使用自己的網路應用裝置（例如，Vyatta 閘道或其他負載平衡器）來控制 VPC 中的任何資源。與此類似，您也無法在 {{site.data.keyword.cloud_notm}} 標準基礎架構中使用自己的負載平衡器或安全群組服務來控制 {{site.data.keyword.vpc_short}} 中的資源。
* 公用閘道不允許將資料流量從網際網路起始至 IBM Cloud VPC 中的 VSI。基於該目的，必須使用「浮動 IP」。
* ACL 是無狀態的，因此必須在 ACL 規則中明確容許傳回的資料流量。

## 此版本中的已知問題
{: #known-issues-in-this-release}

在符合下列所有狀況的情況下，{{site.data.keyword.block_storage_is_short}} 可能無法準確驗證磁區名稱的唯一性：

* 佈建要求是並行的
* 磁區有相同的名稱
* 磁區位於相同地區

## 作為測試版服務提供的功能

與應用裝置安全地建立對等連接作為測試版服務提供。如需與多種類型的應用裝置建立對等連接的文件，請參閱：

* [遠端 Vyatta 同層級](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-vyatta-peer)
* [遠端 StrongSwan 同層級](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-strongswan-peer)
* [遠端 FortiGate 同層級](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-fortigate-peer)
* [遠端 Juniper vSRX 同層級](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-juniper-vsrx-peer)
* [遠端 Cisco ASAv 同層級](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-cisco-asav-peer)
