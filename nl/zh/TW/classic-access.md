---
copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: vpc, classic, access, API, CLI, limitations

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# 設定從 VPC 對標準基礎架構的存取權
{: #setting-up-access-to-your-classic-infrastructure-from-vpc}
[comment]: # (鏈結的說明主題)

您可以為任何帳戶，設定從每個地區中的某個 VPC 對「{{site.data.keyword.cloud}} 標準基礎架構」的存取權（包括 Direct Link 連線功能）。這些特殊的「標準存取 VPC」使用與 {{site.data.keyword.cloud}} 標準基礎架構相同的遞送功能（隱含路由器）。讀者應該已經熟悉標準基礎架構網路連線功能。

設定 VPC 進行標準存取時，標準帳戶中沒有公用介面的每個運算主機（VSI 或 Bare Metal Server）都可以將封包傳送至標準存取 VPC，或從其接收封包。不過，請記住，防火牆、閘道、「網路 ACL」或安全群組可以過濾此資料流量。最佳作法是建議您僅容許應用程式正常運作所需的資料流量。

在具有公用介面的主機中，必須新增一條連回具有標準功能之 VPC 的路徑。
{: important}

## 必要條件：
{: #vpc-prerequisites}

1. 標準帳戶必須鏈結至 IBM Cloud 帳戶。如需這項作業作法的指示，請參閱[鏈結 IBM ID 帳戶](/docs/account?topic=account-unifyingaccounts)。
1. 必須對 VRF 啟用標準帳戶。
    * 如果您的帳戶已有 Direct Link，表示您已備妥。
    * 如果您的帳戶未啟用 VRF 功能，請開立問題單以針對您的帳戶要求「VRF 移轉」。請參閱 Direct Link 文件中的[如何起始轉換](/docs/infrastructure/direct-link?topic=direct-link-how-you-can-initiate-the-conversion#how-you-can-initiate-the-conversion)，以進一步瞭解轉換處理程序。

防火牆、閘道、「網路 ACL」或安全群組可以過濾「標準」與 VPC 資源之間的部分或所有網路資料流量。
{: important}

標準存取 VPC 中的所有子網路，將會共用到標準基礎架構 VRF，它使用 `10.0.0.0/8` 空間中的 IP 位址。為了避免 IP 位址衝突，在標準存取 VPC 中建立子網路時，**請勿使用**位於 `10.0.0.0/8` 空間上的 IP 位址。
{: important}

## 建立標準存取 VPC
{: #create-a-classic-access-vpc}

您可以使用「使用者介面」、「指令行介面 (CLI)」或「應用程式設計介面 (API)」，來建立具有「標準存取」功能的 VPC。

VPC 在建立時必須設定進行「標準存取」：您無法更新 VPC 來新增或移除「標準存取」功能。
{: note}

### 從使用者介面建立標準存取 VPC
{: #create-a-classic-access-vpc-from-the-user-interface}

從**建立 VPC** 頁面建立「標準存取 VPC」，方法是按一下**標準存取**標籤下標題為**啟用對標準資源的存取權**的勾選框。

![classic-access-ui](/images/classic-access-ui.png)

### 使用 CLI 建立標準存取 VPC
{: #create-a-classic-access-vpc-using-the-cli}

建立 VPC 時，請使用 `--classic-access` 旗標。例如，

```
ibmcloud is vpc-create my-access-vpc --classic-access
```
{: pre}


### 使用 API 建立標準存取 VPC
{: #create-a-classic-access-vpc-using-the-api}

建立 VPC 時，傳入 `classic_access` 參數。例如，

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=2019-05-31&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### 標準存取 VPC 預設位址字首
{: #classic-access-default-address-prefixes}

標準存取 VPC 中的所有子網路，將會共用到標準基礎架構 VRF，它使用 `10.0.0.0/8` 空間中的 IP 位址。為了避免 IP 位址衝突，在標準存取 VPC 中建立子網路時，**請勿使用**位於 `10.0.0.0/8` 空間上的 IP 位址。
{: important}

建立新的標準存取 VPC 時，會根據地區和區域來指派預設位址字首，如下表中所示：

區域|位址字首
---------------|---------------
`us-south-1`   | `172.16.0.0/18`
`us-south-2`   | `172.16.64.0/18`
`us-south-3`   | `172.16.128.0/18`
`us-east-1`    | `172.17.0.0/18`
`us-east-2`    | `172.17.64.0/18`
`us-east-3`    | `172.17.128.0/18`
`eu-gb-1`      | `172.18.0.0/18`
`eu-gb-2`      | `172.18.64.0/18`
`eu-gb-3`      | `172.18.128.0/18`
`eu-de-1`      | `172.19.0.0/18`
`eu-de-2`      | `172.19.64.0/18`
`eu-de-3`      | `172.19.128.0/18`
`jp-tok-1`     | `172.20.0.0/18`
`jp-tok-2`     | `172.20.64.0/18`
`jp-tok-3`     | `172.20.128.0/18`
`au-syd-1`     | `172.21.0.0/18`
`au-syd-2`     | `172.21.64.0/18`
`au-syd-3`     | `172.21.128.0/18`


## 限制
{: #vpc-limitations}

* 只有專用（在舊文件中稱為「後端」）網路才會連接至帳戶的專用隱含路由器。
* 只有配置標準 API 的子網路，才會連接至專用隱含路由器的標準端。隱含路由器的遞送功能在標準 VLAN 上的子網路之間無法運作。
* 只可以啟用每個帳戶每個地區 1 個 VPC 進行「標準存取」。

根據您在標準 VSI 或 Bare Metal Server 上所安裝的作業系統，配置程序將有所不同。如需相關資訊，請參閱[關於公用虛擬伺服器](https://cloud.ibm.com/docs/vsi?topic=virtual-servers-about-public-virtual-servers)。
{: note}
