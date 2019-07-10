---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: glossary, terminology, definition, access, floating, geography, image, region, zone, instance, VSI, LBaaS, VPN, VPC, NAT, profile, resource, security group, shares, storage, VNPaaS, volume, subnet, SSH, key, gateway, ACL

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

# VPC 名詞解釋
{: #vpc-glossary}

下列術語通用且專屬於 {{site.data.keyword.cloud}} Virtual Private Cloud。請參閱 [IBM Cloud 名詞解釋](/docs/overview/glossary?topic=overview-glossary)，以瞭解更多術語。

## 存取控制清單 (Access Control List)
{: #access-control-list}

「存取控制清單 (ACL)」提供[子網路](#subnet)入埠及出埠資料流量的安全控制。ACL 是無狀態的。每一個 ACL 都由基於_來源 IP_、_來源埠_、_目的地 IP_、_目的地埠_ 和_通訊協定_ 的規則組成。

在 IBM Cloud VPC 中，每個子網路都有建立預設 ACL（容許入埠及出埠資料流量），但客戶可以建立自訂 ACL。任何時候都只能有一個 ACL 連接至一個子網路，但一個 ACL 可以連接至多個子網路。

有時也稱為_網路 ACL_ 或 NACL。

## 浮動 IP (Floating IP)
{: #floating-ip}

「浮動 IP」這種方法使用儲存區中的指派_浮動 IP 位址_，針對 VPN 資源（例如實例、負載平衡器或 VPN 通道）提供網際網路的入埠及出埠存取權。

「浮動 IP 位址」是系統從預先存在的儲存區中所提供的公用 IP 位址。您不能使用自己的公用 IP 位址。可以從網際網路存取「浮動 IP 位址」，而且它們可以透過 vNIC 與實例（例如，VSI、負載平衡器或 VPN 閘道）相關聯。

您可以從 IBM 所提供的可用「浮動 IP 位址」儲存區中保留一個「浮動 IP 位址」，您可以將它與相同 Virtual Private Cloud 中的任何實例建立關聯或取消關聯。浮動 IP 利用 1 對 1 [NAT](#nat)，以容許伺服器與公用網際網路以及雲端環境內的專用子網路進行通訊。

## 地理位置 (Geography)
{: #geography}

在 VPC 中，地理位置指定會提供在其中部署 VPC 及其關聯資源的地區及區域的相關資訊。

## 映像檔 (Image)
{: #image}

啟動虛擬伺服器實例或_實例_ 所需的資訊。一般而言，它是商業可用作業系統的 Snapshot 映像檔，用於開機。

## 隱含路由器 (Implicit Router)
{: #implicit-router}

在 VPC 中建立的所有子網路之間的固有網路連線功能。

## 實例 (Instance)
{: #instance}

在 VPC 內執行的虛擬伺服器或 VSI 的簡稱。

## LBaaS
{: #lbaas}

「負載平衡器即服務 (LBaaS)」可讓您以平衡配置方式將資料流量分散到 VPC 中的實例。

## NAT
{: #nat}

「網址轉換 (NAT)」是[網際網路 RFC 1631 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://tools.ietf.org/html/rfc1631){: new_window} 中所述的定址方法，因此，一個 IP 位址可以用來與數個其他 IP 位址進行通訊（例如，專用子網路上的 IP 位址），基本上是依據參考表。NAT 有兩種主要類型：1 對 1 NAT 及[多對 1 NAT ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Network_address_translation){: new_window}。NAT 最初是作為延伸 IPv4 IP 位址有效生命週期的方法，但現在通用於雲端環境，作為建立與專用子網路通訊的方法。

## 設定檔 (Profile)
{: #profile}

「設定檔」是一種通用的 vCPU 與 RAM 組合，可以快速將其實例化，以啟動虛擬伺服器實例 (VSI)。支援三個設定檔系列：「平衡」、「運算」及「記憶體」。


## 公用閘道 (Public Gateway)
{: #public-gateway}

「公用閘道 (PGW)」會啟用子網路（所有 VSI 都連接至子網路）的出埠專用存取權，用來連接至網際網路。請注意，依預設，子網路是專用的；不過，您可以選擇性地建立 PGW，並將子網路連接至 PGW。子網路連接至 PGW 之後，該子網路中的所有 VSI 就可以連接至網際網路。

PGW 使用「多對 1 NAT」，這表示含有專用位址的數千個 VSI 將使用一個公用 IP 位址與公用網際網路進行交談。PGW 不會讓網際網路起始與那些實例的連線。

## 地區 (Region)
{: #region}

在其中部署 VPC 的地理區域。每個地區包含多個區域，分別代表獨立錯誤網域。IBM Cloud VPC 可跨越其指派地區內的多個區域。

## 資源 (Resource)
{: #resource}

屬於 VPC 部署且可能產生計費的資產類型，例如 VSI、浮動 IP 或 VPC 本身。資源可以結合到_資源群組_，以在 VPC 中搭配使用特定資源時，更容易進行統計作業。

## 安全群組 (Security Group)
{: #security-group}

安全群組可用來作為虛擬防火牆，以控制一個以上伺服器 (VSI) 的入埠及出埠資料流量。安全群組是規則集合，其指定是否容許相關聯 VSI 的資料流量。當客戶啟動 VSI 時，客戶可以建立一個以上安全群組與該 VSI 的關聯。如果有正確的許可權，客戶即可修改安全群組規則。

## 共用 (Share)
{: #shares}

在 VPC 中提供檔案儲存空間的方式。

## 子網路 (Subnet)
{: #subnet}

子網路是指 IP 位址範圍，會連結至單一區域，不能跨越多個區域或地區。子網路可以跨越 IBM Cloud VPC 中的整體區域。

基於 IBM Cloud VPC 的目的，子網路的重要特徵是子網路彼此隔離且透過一般方式交互連接的事實。「網路存取控制清單 (ACL)」可以完成子網路隔離，用來作為防火牆以控制子網路之間的資料封包流程。同樣地，安全群組可用來作為虛擬防火牆，以控制出入個別虛擬伺服器實例 (VSI) 的資料封包流程。

這是子網路的隔離，可讓您在公用雲端內建立專用空間。

## SSH 金鑰 (SSH Key)
{: #ssh-key}

自動產生的加密存取認證，可控制對 VPC 中虛擬伺服器實例的存取權。

## 磁區 (Volume)
{: #volumes}

VPC 中用於虛擬伺服器實例的 Hypervisor 裝載的高效能資料儲存空間。

## VPC
{: #vpc}

與帳戶相關聯的虛擬網路。它提供對虛擬基礎架構及網路資料流量分段的細部控制，以及安全和動態調整的能力。

## VPN
{: #vpn}

「虛擬專用網路 (VPN)」容許兩個端點之間的專用連線，即使是跨公用網路傳送資料。客戶可以像連接至專用網路一樣地共用資料。通常，VPN 會與安全方法（例如鑑別及加密）一起使用，以提供最大資料安全及隱私權。

## VPN 連線 (VPN Connection)
{: #vpn-connection}

兩個端點之間維持專用且可加密的專用連線，即使是跨公用網路傳送資料。

## VPNaaS
{: #vpnaas}

VPN 即服務 (VPNaaS) 允許透過 VPC 內部和外部的端點設定 VPN。

## 區域 (Zone)
{: #zone}

獨立錯誤網域。「區域」是一種抽象概念，用來協助改良容錯及減少延遲。「區域」可保證下列內容：

 * 因為每個區域都是一個獨立的錯誤網域，所以一個地區中的兩個區域不太可能同時失敗。
 * 一個地區中兩個區域之間的資料流量的延遲會少於 2 毫秒。
