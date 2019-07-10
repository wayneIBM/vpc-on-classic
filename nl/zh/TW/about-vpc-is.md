---
copyright:
  years: 2017, 2019
lastupdated: "2019-05-29"
keywords: features, benefits, isolation, provisioning, security, cloud-native, workloads, BYOIP, vpc

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# 關於 Virtual Private Cloud
{: #about}

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) 是下一代 IBM Cloud 平台。透過 VPC，您可以在 IBM Cloud 中建立自己的空間，以在公用雲端中執行隔離環境。VPC 讓您既擁有專用雲端的安全，也具備公用雲端的敏捷性和易用性。

可以根據需要管理網路服務和啟動實例，以支援關鍵任務、雲端容錯和雲端原生應用程式。可用的主要功能包括：

* 透過 API 建立及管理隔離的應用程式環境
* 定義您自己的網路連線功能原則，以實現安全和方便存取
* 使用 BYOIP（自帶 IP）來設計網路拓蹼
* 佈建資源並將其相互連接，也可以將資源相互隔離
* 涵蓋多個地區以進行災難回復及備援
* 使用容許跨地區之高速且低延遲連線的可用性區域，並具有 HA
* 利用高速網路及儲存裝置
* 容許「一律開啟」服務（控制平面）
* 提供並利用核心服務：IPAM、VPN、防火牆、SSH、DNS 及「L4 負載平衡」
* 設定其他服務：自動調整大小、CDN、協力廠商 NFV

下面的「VPC 概觀圖」說明可用的 VPC 元件以及這些元件如何協作來為您提供可能需要的服務和連線功能。在深入瞭解提供每個元件更多相關詳細資料的主題時，可返回參照此圖。

![IBM Cloud VPC 概觀](images/vpc-experience-simple.svg "IBM Cloud VPC 概觀"){: caption="圖：IBM Cloud VPC 概觀" caption-side="top"}

下列各區段提供了 IBM Cloud VPC 中可用特性的概觀。

## 網路功能

{{site.data.keyword.cloud_notm}} VPC 提供了全面、易用的網路連線功能，包括能夠建立[全球可用的多區域地區](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region)中的多個虛擬專用雲端、不同區域中的子網路、[IP 位址範圍選擇](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)、虛擬防火牆（[安全群組](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)和[網路 ACL](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls)）、站台對站台虛擬專用網路 ([VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)) 以及具有彈性的負載平衡 ([LBaaS](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc))。

VPC 分成子網路，並使用某範圍的專用 IP 位址。不過，依預設，相同 VPC 內的所有資源（如 VSI）都可以彼此通訊，不論其子網路為何。子網路包含在單一區域中，不能跨多個區域，這有助於確保安全，還有利於減少延遲並擁有高可用性。

使用建議的字首範圍和預配置的網路安全原則，可輕鬆建立多個 VPC 和子網路，或者設計並定義您自己的位址字首和自訂安全原則。

CIDR 區塊 161.26/16 和 166.8/14 都保留並遞送到每個子網路中。請閱讀有關[可用於 IBM Cloud VPC 的服務端點](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)的資訊。此外，CIDR 區塊 10.240/13 保留用於 VPC 位址字首，並[以預先定義方式](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#ibm-cloud-vpc-and-address-prefixes)在區域之間進行劃分，無法對此區塊進行變更。
{: important}

### 網際網路存取

有三個選項可用於啟用從虛擬伺服器實例 (VSI) 到公用網際網路的通訊：
* 使用公用閘道 (PGW)，讓所連接子網路上全部的虛擬伺服器實例能與網際網路通訊。使用 PGW 無需付費，但若使用頻寬則不在此限。
* 使用浮動 IP (FIP)，讓單一虛擬伺服器實例 (VSI) 與網際網路之間能相互通訊。
* 使用 VPN 閘道。如需相關資訊，請參閱 [VPN for VPC 文件](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc#--using-vpn-with-your-vpc)。
{: note}

### 標準存取

現有 {{site.data.keyword.cloud_notm}} 基礎架構使用者可以使用[標準存取](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)將其標準基礎架構（包括 Direct Link 連線功能）連接至每個地區中的一個 VPC。

### BYOIP

您可以將自己的公用 IPv4 位址範圍 (BYOIP) 帶至您的 IBM Cloud VPC 帳戶。

當您使用 BYOIP 時，{{site.data.keyword.cloud_notm}} 必須在 {{site.data.keyword.cloud_notm}} 資源上配置這些 IPv4 位址，以便在您提供的位址上傳送及接收封包。因此，在 {{site.data.keyword.cloud_notm}} 上使用您提供的 IPv4 範圍後，這些 IP 位址可能會對 IBM 的支援人員和第三個方公開。
{: important}

您必須使用 API 或 CLI 來使用 BYOIP。此功能在 IBM Cloud 主控台使用者介面中無法使用。
{: note}

### 廣域連線功能

您可以將應用程式及可用資源的範圍設為跨越多個地區。您可以使用 [VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)，為混合式雲端部署的其他專案及其他部分建立專用連線。

可輕鬆地擴充及共用 VPC 網路與資源，以從內部部署機能延伸到您的雲端。

## 安全

在 {{site.data.keyword.cloud_notm}} VPC 中，透過[安全群組](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)和[網路存取控制清單](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) (ACL) 整合了安全；其中，安全群組充當虛擬防火牆，提供實例層次的保護，而 ACL 提供子網路層次的保護。

## 高可用性

{{site.data.keyword.cloud_notm}} Cloud Virtual Private Cloud (VPC) 可讓您的應用程式與所有地區中的其他網路進行邏輯隔離，同時提供可調整性及安全。使用[負載平衡器](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)可在一組目標之間分散網路資料流量，以提高效能和 HA。「負載平衡器」也會監視應用程式及服務的性能。您可以設定負載平衡器，以將送入的應用程式資料流量分散到單一區域中的實例，或某個地區內的多個區域。

## 運算功能

使用 [IBM Cloud Virtual Servers for Virtual Private Cloud](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-virtual-private-cloud) 可在 IBM Cloud 中佈建可調整運算資源。使用針對特定工作負載最佳化的預定義[設定檔](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles)，可快速建立虛擬伺服器實例 (VSI)。佈建多網卡、[多 vNIC](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-network-security-options) 實例時，請從支援的庫存[映像檔](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-images)中進行選擇或上傳自訂映像檔。

{{site.data.keyword.cloud_notm}} VPC 新增了一個網路編排層，用於消除虛擬伺服器實例的 pod 界限，以便能為了調整實例而建立無限容量。網路編排層處理不同地區和區域的 IBM Cloud VPC 中所有虛擬伺服器實例的所有網路連線功能。透過 {{site.data.keyword.cloud_notm}} VPC 提供的軟體定義網路連線功能，您有 VPN、LBaaS 和多 vNIC 實例的更多選項可選擇，並且能使用更大的子網路大小。

## 儲存空間功能

佈建 {{site.data.keyword.cloud_notm}} Virtual Servers for Virtual Private Cloud 實例時，會建立一個 100 GB 的通用 IOPS (3 IOPS/GB) 區塊儲存空間磁區作為主要開機磁區，並連接到該實例。在 VPC 網路中佈建虛擬伺服器實例或建立獨立於 VSI 生命週期的新磁區時，可以建立區塊儲存空間磁區。

為 VPC 佈建更多區塊儲存空間時，可以選取用於區塊儲存空間磁區的 [IOPS 層級](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#tiers)，也可以指定[自訂 IOPS 設定檔](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#custom)。

## 針對雲端原生和混合工作負載設計

VPC 是雲端原生工作負載的理想選擇，用於將現有基礎架構鏈結到 {{site.data.keyword.cloud_notm}}。您可以為各種工作負載（例如，認知、AI 和機器學習計算）建立最佳雲端。

以下是 VPC 支援混合式雲端工作負載、雲端容錯工作負載和雲端原生工作負載的更多方法：

* 具有水平自動調整功能的負載平衡
* 監視和性能檢查
* 支援 GPU
* 佈建 VSI（具有親緣性和反親緣性群組）

## 進一步瞭解

* [**VPC 術語**](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-glossary)
* [**VPC 安全**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-security-in-your-ibm-cloud-vpc#security-in-your-ibm-cloud-vpc)
* [**可用的 VPC 地區和子網路**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)
* [**在 VPC 中建立及管理網路資源**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-network-resources-in-vpc)
* [**在 VPC 中建立和管理虛擬伺服器實例**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-virtual-server-instances)
* [**在 VPC 中建立和管理區塊儲存空間**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-storage-in-vpc)
