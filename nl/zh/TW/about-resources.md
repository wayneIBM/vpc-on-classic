---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, load balancer, VPN, operator, editor, viewer, admin

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

# 關於 VPC Infrastructure 資源
{: #about-vpc-infrastructure-resources}

_資源_ 一詞指的是 {{site.data.keyword.cloud}} Virtual Private Cloud 部署的元件部分。每個 VPC 都可以包含下列類型的資源，而這些資源可以視需要進行分組：

* **vpc**
* **實例**
* **金鑰**
* **映像檔**
* **子網路**
* **磁區**
* **網路存取控制清單 (ACL)**
* **安全群組**
* **浮動 IP**
* **vnic**
* **閘道**
* **負載平衡器**
* **vpn**

一些 VPC 基礎架構資源會從母項 VPC 繼承其授權原則，用於管理自身的使用情況，但另一些資源是個別管理的。

目前，`instance`、`key`、`security-group`、`volume`、`loadbalancer` 和 `VPN` 資源類型會維護其自己的授權原則。本文件稍後會提供有關這些資源的資源授權詳細資料。
{: note}

## 資源原則
{: #resource-policies}

一般而言，VPC 中資源的存取權是透過_原則_ 所控制。如果您要自訂 VPC 資源的存取權，則可以建立自訂的原則。資源原則可以將目標設為：

* 所有資源
* 某類型的所有資源
* 群組中的所有資源
* 個別資源

## 資源及資源群組
{: #resources-and-resource-groups}

_資源群組_ 是基於建立授權和使用而建立關聯的資源集合（例如整個 VPC 或是單一子網路及其 ACL）。您可以將資源群組視為可能由專案、部門或團隊使用的基礎架構集合。

可以使用搜尋 CLI 來顯示連接到資源的標籤。透過適當的過濾器，可以僅顯示您在乎的實例，例如 `ibmcloud resource search name:` 或 `ibmcloud resource search 'service_name:is AND type:vpc'`。
{: tip}

大型企業可能希望將一個 VPC 分割成數個資源群組。較小的公司可能不需要將其 VPC 分割為數個資源群組，因為其團隊全部都會使用整個 VPC。如果您熟悉 _OpenStack_，則資源群組的概念類似 _OpenStack Keystone_ 中的_專案_。

「只」有在建立 VPC 時，才能將資源指派給資源群組。建立資源群組之後，資源就無法變更資源群組。
{: .note}

資源群組主要是針對大型組織所設計。對於大部分公司及團隊而言，資源分組位於 VPC 界限上。可能將個別 VSI（實例）指派給資源群組，以及可能將個別使用者指派給 VSI（實例）資源時，此一般經驗法則可能會變更。

當您設定 IBM Cloud VPC 時，如果要使用資源群組，則最好針對如何將資源以及組織中的使用者指派給每個資源群組，事先有所計畫。
{: tip}

## VPC 的特性
{: #specifics-for-vpc}

一些 VPC 基礎架構資源會從帳戶或母項 VPC 繼承其授權原則，用於管理自身的使用情況，而另一些資源具有個別的原則。

### 依資源類型的 VPC 資源授權
{: #vpc-resource-authorization-by-resource-type}

此表格彙總預設情況下 VPC 資源的授權方式。

| 資源類型 | 其預設授權的取得位置 |
|-----------|------------|
| VPC | 建立時的授權檢查 |
| 負載平衡器 | 建立時的授權檢查 |
| VPN | 建立時的授權檢查 |
| 子網路 | 母項 VPC |
| 公用閘道 | 母項 VPC |
| 實例 | 建立時的授權檢查 |
| 安全群組 | 建立時的授權檢查 |
| 金鑰 | 建立時的授權檢查 |
| 映像檔 | 帳戶 |
| 浮動 IP | 帳戶 |
|網路 ACL | 帳戶 |
| 磁區 | 建立時的授權檢查 |

如果未指派，則可以在帳戶層次的範圍內建立「浮動 IP」及網路 ACL。不過，只要將浮動 IP 指派給實例，或將 ACL 指派給子網路，這些資源就會變成遵照 VPC 的授權範圍。
{: note}

### 資源的角色及授權動作的 VPC 涵蓋範圍
{: #vpc-coverage-of-roles-and-authorized-actions-on-resources}

在具有 2 個 VPC 的情境中，以下這些範例說明任何具有特定角色的使用者嘗試執行特定動作時會發生的情況：

* 使用者 _admin_，具有所有資源的**管理者**許可權
  * 建立 `vpc1`：正常

* 使用者 _editor_，具有所有資源的**編輯者**許可權
  * 建立 `vpc2`：正常
  * 更新 `vpc2`：正常
  * 刪除 `vpc2`：正常

* 使用者 _operator_，具有所有資源的**操作員**許可權
  * 建立 `vpc`：拒絕
  * 更新 `vpc1`：拒絕
  * 刪除 `vpc1`：拒絕

* 使用者 _viewer_，具有所有資源的**檢視者**許可權
  * 列出 VPC：看到 [vpc1]
  * 讀取 `vpc1`：正常

* 使用者 _noRole_，沒有任何原則
  * 列出 VPC：看到 []
  * 讀取 `vpc1`：拒絕

### VPC 涵蓋範圍摘要表格
{: #vpc-coverage-summary-table}

對於任何依角色（「管理者」、「編輯者」、「操作員」、「檢視者」）授與存取權的原則，提供下列動作（X=拒絕、o=正常）

 角色：     | 無   | 檢視者 | 操作員| 編輯者 | 管理者
-----------:|------|--------|-------|--------|-------
建立        | X    | X    | X     | o      | o
列出        | X    | o      | X     | o      | o
讀取        | X    | o      | X     | o      | o
更新        | X    | X      | X     | o      | o
刪除        | X    | X      | X     | o      | o


## VPN for VPC 的資源授權
{: #resource-authorizations-of-vpn-for-vpc}

**VPN for VPC** 的資源授權與其他類型的 VPC 資源授權分開設定，但方式類似。

VPN for VPC 中的原則控制對 VPN 資源（特別是 VPN 閘道）的存取權。要存取 VPN 閘道的子項資源（例如，VPN 連線以及本端或同層級 CIDR），您必須對母項 VPN 閘道具有**讀取**權（或更多存取權）。資源原則只能用於對母項 VPN 閘道具有**讀取**權或更高存取權的 VPN 連線。VPN 中的資源原則可以將目標設為：

* 所有 VPN 資源（VPN 閘道、VPN 連線、IKE 和 IPsec 原則）
* 資源群組中的所有 VPN 資源
* 個別 VPN 閘道

VPN 用量也會分別計費。
{: note}

### 資源的角色及授權動作的 VPN for VPC 涵蓋範圍
{: #vpn-for-vpc-coverage-of-roles-and-authorized-actions-on-resources}

支援的使用者角色與 VPC 資源授權中支援的使用者角色相同，但針對每個角色啟用不同的動作。

以下這些範例說明具有特定角色的使用者嘗試執行特定 VPN 相關動作時會發生的情況：

* 使用者 _admin_，具有所有資源的**管理者**許可權
  * 建立、更新、刪除、讀取、列出「VPN 閘道」：正常
  * 建立、更新、刪除、讀取、列出「VPN 連線」：正常
  * 建立、更新、刪除、讀取、列出「VPN 連線對等節點及本端 CIDR」：正常
  * 建立、更新、刪除、讀取、列出「IKE 原則」：正常
  * 建立、更新、刪除、讀取、列出「IPSec 原則」：正常

* 使用者 _editor_，具有所有資源的**編輯者**許可權
  * 與「管理者」的作業相同

* 使用者 _operator_，具有所有資源的**操作員**許可權
  * 建立、更新、刪除「VPN 閘道」：拒絕
  * 建立、更新、刪除「VPN 連線」：拒絕
  * 建立、更新、刪除「VPN 連線對等節點及本端 CIDR」：拒絕
  * 建立、更新、刪除「IKE 原則」：拒絕
  * 建立、更新、刪除「IPSec 原則」：拒絕
  * 讀取、列出「VPN 閘道」：正常 - 看到實際資料
  * 讀取、列出「VPN 連線」：正常 - 看到實際資料
  * 讀取、列出「VPN 連線對等節點及本端 CIDR」：正常 - 看到實際資料
  * 讀取、列出「IKE 原則」：正常 - 看到實際資料
  * 讀取、列出「IPSec 原則」：正常 - 看到實際資料

* 使用者 _viewer_，具有所有資源的**檢視者**許可權
  * 與「操作員」的作業相同

* 使用者 _noRole_，沒有任何原則
  * 讀取和列出 VPN 閘道：拒絕 - 清單顯示空的資料
  * 讀取和列出 VPN 連線：拒絕
  * 讀取和列出 VPN 連線同層級和本端 CIDR：拒絕
  * 讀取和列出 IKE 原則：拒絕 - 清單顯示空的資料
  * 讀取和列出 IPsec 原則：拒絕 - 清單顯示空的資料

### VPN for VPC 涵蓋範圍摘要表格
{: #vpn-for-vpc-coverage-summary-table}

透過下表，可以將 VPN for VPC 中的動作角色對映視覺化（X=拒絕、o=正常）：

 角色：     | 無   | 檢視者 | 操作員| 編輯者 | 管理者
-----------:|------|--------|-------|--------|-------
建立        | X    | X      | X    | o      | o
列出        | X    | o      | o     | o      | o
讀取        | X    | o      | o     | o      | o
更新        | X    | X      | X     | o      | o
刪除        | X    | X      | X     | o      | o

## Load Balancer for VPC 的資源授權
{: #resource-authorization-for-load-balancer-for-vpc}

**Load Balancer for VPC** 用量的資源授權與 VPC 中的其他資源授權分開設定，但方式類似。

「負載平衡器」用量也會分別計費。
{: note}

### 資源的角色及授權動作的負載平衡器涵蓋範圍
{: #load-balancer-coverage-of-roles-and-authorized-actions-on-resources}

以下這些範例說明具有特定角色的使用者嘗試執行與「VPC 負載平衡器」相關的特定動作時會發生的情況：

* 使用者 _admin_，具有所有資源的**管理者**許可權
  * 建立、更新、刪除、讀取、列出「負載平衡器」：正常
  * 建立、更新、刪除、讀取、列出「接聽器」：正常
  * 建立、更新、刪除、讀取、列出「儲存區」：正常
  * 建立、更新、刪除、讀取、列出「成員」：正常
  * 建立、更新、刪除、讀取、列出「負載平衡器統計資料」：正常

* 使用者 _editor_，具有所有資源的**編輯者**許可權
  * 與「管理者」的作業相同

* 使用者 _operator_，具有所有資源的**操作員**許可權
  * 建立、更新、刪除、讀取、列出「負載平衡器」：拒絕
  * 建立、更新、刪除、讀取、列出「接聽器」：拒絕
  * 建立、更新、刪除、讀取、列出「儲存區」：拒絕
  * 建立、更新、刪除、讀取、列出「成員」：拒絕
  * 建立、更新、刪除、讀取、列出「負載平衡器統計資料」：拒絕

* 使用者 _viewer_，具有所有資源的**檢視者**許可權
  * 讀取、列出「負載平衡器」：正常
  * 讀取、列出「接聽器」：正常
  * 讀取、列出「儲存區」：正常
  * 讀取、列出「成員」：正常
  * 讀取「負載平衡器統計資料」：正常

* 使用者 _noRole_，沒有任何原則
  * 讀取、列出「負載平衡器」：拒絕 - 清單顯示空的資料
  * 讀取、列出「接聽器」：拒絕
  * 讀取、列出「儲存區」：拒絕
  * 讀取、列出「成員」：拒絕
  * 讀取「負載平衡器統計資料」：拒絕

### 負載平衡器涵蓋範圍摘要表格
{: #load-balancer-coverage-summary-table}

透過下表，可以將 Load Balancer for VPC 中的動作角色對映視覺化（X=拒絕、o=正常）：

 角色：     | 無   | 檢視者 | 操作員| 編輯者 | 管理者
-----------:|------|--------|-------|--------|-------
建立        | X    | X      | X     | o      | o
列出        | X    | o      | X     | o      | o
讀取        | X    | o      | X     | o      | o
更新        | X    | X      | X     | o      | o
刪除        | X    | X      | X     | o      | o


## Virtual Server for VPC 的資源授權
{: #planning-virtual-servers-for-vpc-permissions}

**Virtual Server for VPC** 的資源授權與 VPC 中的其他資源授權分開設定。 

Virtual Server for VPC 用量將單獨計費。
{: note}

* 身為管理者，您可以定義角色以及對 {{site.data.keyword.vsi_is_short}} 執行任何可用的動作。
* 身為編輯者，您可以修改狀態以及建立或刪除子資源。
* 身為操作員，您可以執行不變更資源狀態的動作。
* 身為檢視者，您可以執行不變更資源狀態的動作。

### 虛擬伺服器涵蓋範圍摘要表格
{: #vsi-coverage-summary-table}

透過下表，可以將 Virtual Server for VPC 中的動作角色對映視覺化（X=拒絕、o=正常）：

 角色：     | 無   | 檢視者 | 操作員| 編輯者 | 管理者
-----------:|------|--------|-------|--------|-------
建立        | X    | X      | X     | o      | o
列出        | X    | o      | o     | o      | o
讀取        | X    | o      | o     | o      | o
更新        | X    | X      | X     | o      | o
刪除        | X    | X      | X     | o      | o

建立實例時，如果指定了 VPC 和安全群組資源，則您也必須具有這些資源的操作員存取權。子網路和浮動 IP 資源從關聯的 VPC 繼承許可權。  
{: tip} 
