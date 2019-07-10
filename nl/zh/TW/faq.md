---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: FAQ, faqs, limit, resource, vNIC, VSI, PGW, console, VRF, bandwidth, COS, egress, load balancer

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
{:faq: data-hd-content-type='faq'}


# 常見問題
{: #faqs}

## 可以將 VPC 連接至其他 IBM Cloud 工作負載嗎？  
{: #faq-0}
{:faq}

可以，您可以在每個地區中設定一個 VPC 對 {{site.data.keyword.cloud}} 標準基礎架構的存取權。如需相關資訊，請參閱[設定對標準基礎架構的存取權](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)。

## VPC 名稱中的字元數限制為何？
{: #faq-1}
{:faq}

目前，限制是 100。如果超出這個限制，您可能會收到「內部錯誤」訊息。

## 我的任何 VPC 資源名稱開頭可以是數字嗎？
{: #faq-2}
{:faq}

不可以，雖然名稱可以包含數字，但它必須以字母開頭。

## 我可以在名稱中使用的字元是否有任何限制？
{: #faq-3}
{:faq}

有，使用者介面會封鎖連續的兩個雙橫線、底線及句點成為 VSI 名稱的一部分。

## 可以建立沒有子網路而只有「浮動 IP」的 VSI 嗎？
{: #faq-9}
{:faq}

否。

## VSI 可以連接至多個 VPC 嗎？
{: #faq-10}
{:faq}

否。

## 在 VPC 中建立子網路之後，是否可以變更子網路大小？
{: #faq-11}
{:faq}

否。

## 從 VPC 到 COS 的資料流量是否有輸出頻寬費用？
{: #faq-17}
{:faq}

只要 ADN 端點用來從 VPC 連接至 COS，但仍套用 API 呼叫費用，就沒有任何輸出頻寬費用。

 ## 設定 VPN for VPC 時，為何需要指定子網路，我應該指定什麼？
{: #faq-16}
{:faq}

當您設定 VPN 閘道時，必須指定子網路，以讓 VPN 閘道可以取得 VPN 服務所需的 IP 位址。透過指定子網路，您仍可以控制 VPN 資料流量的處理方式，而且可以靈活地指定更接近利用 VPN 之資源的位置。如此一來，您可以減少延遲並改善效能。

## 如何知道將在哪裡部署負載平衡器實例？
{: #faq-18}
{:faq}

所選擇的子網路決定要部署 VPC 負載平衡器的位置。VPC 負載平衡器是地區資源。最佳作法是從給定地區下的不同區域中選擇子網路，以提高備援。


## 在 VPC 中的「浮動 IP」指派期間，客戶必須指定 VSI 的 vNIC，這是否正確？
{: #faq-4}
{:faq}

是的，實際上，它必須是伺服器的主要網路介面。

不過，如果我們在網路區域的 API 中新增 "address" 資源，我們便大可變更浮動 IP 與位址而非介面的關聯。

## VPC 中虛擬伺服器實例上的 vNIC 可以同時具有浮動 IP 及專用 IP 嗎？
{: #faq-5}
{:faq}
 
是。

## 在 IBM Cloud 主控台的 VPC 使用者介面中，每個子網路有一個「切換開關」用於連接到公用閘道 (PGW) 或從 PGW 分離。誰建立 PGW？在該使用者介面中，客戶要將子網路連接到 PGW 時，PGW 的狀態會為備妥嗎？
{: #faq-6}
{:faq}

PGW 必須透過 API 來明確建立，因為 API 需要子網路與特定公用閘道明確關聯。

## 假設 VPC 中的 VSI1 只有 vNIC1，並連接至 Subnet1。Subnet1 連接至「公用閘道 (PGW)」。客戶仍可以將「浮動 IP」指派給 VSI1 嗎？
{: #faq-7}
{:faq}

是，伺服器可以位於連接至公用閘道的子網路上，也可以具有浮動 IP。將浮動 IP 指派給 VSI 與是否存在連接到子網路的公用閘道無關。VSI 的相關聯浮動 IP 優先順序會高於連接至子網路的公用閘道。

## 在 VPC 中，VSI1 有 2 個 vNIC，稱為 vNIC1 及 vNIC2。這兩個 vNIC 可以連接至相同的子網路嗎？
{: #faq-8}
{:faq}
 
是，不限制連接至相同子網路的相同伺服器上有多個網路介面。但是，不支援一個 VSI 上有多個 NIC。

## 誰強制 VPC 的每個區域只能有 1 個「公用閘道」？
{: #faq-13}
{: faq}

VPC API 強制實施此限制。

## 在建立 PGW 期間，我需要保留 FIP 嗎，還是系統會自動保留 FIP？當我查詢所有「浮動 IP」時，是否會看到「浮動 IP」？
{: #faq-12}
{: #faq}

如果未指定現有浮動 IP，API 會自動建立浮動 IP 以及公用閘道。而且，是的，此清單中會顯示該浮動 IP。


## VRF 對其他網路功能及子網路的影響為何？
{: #faq-14}
{:faq}

VLAN 及 VRF 的警告：

* VRF 環境中不容許帳戶之間的 VLAN Spanning。
* IPSec VPN 服務會受限。

VRF 不會阻止 SSL 或 PPTP VPN 存取，但其行為會變更。沒有 VRF 的情況下，一個 VPN 連線就足以查看您帳戶上的所有伺服器。使用 VRF 時，您只能存取市場中與您的 VPN 相關聯的資源。因此，如果您連接至 DAL VPN，則只能連接至 DAL 伺服器。
{: note}

## 在 VPC 中使用 `instance-network-interface-floating-ip-add` 次指令的正確方式為何？是否預先建立/配置浮動 IP 位址？
{: #faq-15}
{:faq}

 您必須先配置浮動 IP，然後在 interface floating IP `add` 指令上提供 FIP 位址。

 例如：`ibmcloud is floating-ip-reserve FLOATING_IP_NAME (--zone ZONE | --nic NIC_ID)`。提供區域。
