---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, cli, rias, infrastructure

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# 使用 IBM Cloud CLI 刪除 VPC
{: #deleting-using-cli}

本主題提供了有關對於每種 VPC 資源，如何按建議的次序從 {{site.data.keyword.cloud}} Virtual Private Cloud 刪除資源的範例。 

以下是關於刪除作業務必要記住的一些重要事實：

* 在 VPC 是空的之前，無法刪除 VPC。 
* 必須先順利刪除母項資源包含的所有資源，然後才能刪除母項資源。 
* 如果資源處於擱置刪除狀態，則在清單查詢中檢視時，該資源會顯示 `deleting` 狀態。 
* 大多數刪除要求是_非同步_執行的，這意味著資源尚未完全刪除時，可能會在清單查詢中顯示 **deleting** 狀態。 
* 試圖刪除母項資源之前，必須輪詢以確保子項資源不再顯示在清單視圖中。 

資源的狀態從 `deleting` 變更為 `failed` 後，可以重試刪除該資源。如果無法刪除處於 `failed` 狀態的資源，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。
{: tip}

## 步驟 1：尋找要刪除的 VPC 中的所有子網路
{: #deleting-find-subnets-cli}

VPC 的每個子網路都必須先刪除，才能刪除 VPC。透過執行下列指令並查看 `ID` 值，取得要刪除的 VPC 的 ID：

```
ibmcloud is vpcs
```
{: pre}

將 VPC 的 ID 儲存在變數中，以供稍後使用，例如：

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

若要取得帳戶中所有子網路的清單，請執行下列指令：

```
ibmcloud is subnets
```
{: pre}

查看 `VPC` 值以確定需要刪除的子網路。將子網路 ID 儲存在變數中，以便稍後可以使用，例如：

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

如果要刪除的 VPC 有多個子網路，請重複這些步驟以刪除每個子網路。
{: important}

## 步驟 2：刪除 VPC 中的每個子網路
{: #deleting-subnet-resources-cli}

### 刪除子網路中的所有 VPN 閘道（如果有的話）。

若要列出帳戶中所有 VPN 閘道，請執行下列指令：

```
ibmcloud is vpn-gateways
```
{: pre}

針對子網路中您要刪除的每個 VPN 閘道，請執行下列指令，其中 `$vpnid` 是 VPN 閘道的 ID。

```
ibmcloud is vpn-gateway-delete $vpnid
```

VPN 閘道的狀態應該會立即變更為 `deleting`，但該閘道仍會顯示在清單查詢結果中。刪除 VPN 閘道可能需要最多 30 分鐘。
{: note}

等待刪除 VPN 閘道時，可以要求平行刪除其他子網路資源。但是，要到子網路中的 VPN 閘道和其他所有資源不再顯示在清單查詢結果中之後，才能刪除子網路。

### 刪除子網路中的所有負載平衡器（如果有的話）
{: #deleting-lbs-cli}

若要列出帳戶中所有負載平衡器，請執行下列指令：

```
ibmcloud is load-balancers
```
{: pre}

針對子網路中您要刪除的每個負載平衡器，請執行下列指令，其中 `$lbid` 是負載平衡器的 ID。

```
ibmcloud is load-balancer-delete $lbid
```
{: pre}

負載平衡器的狀態應該會立即變更為 `deleting`，但該負載平衡器仍會顯示在清單查詢結果中。刪除負載平衡器可能需要最多 30 分鐘。
{: note}

等待刪除負載平衡器時，可以要求平行刪除其他子網路資源。但是，要到子網路中的負載平衡器和其他所有資源不再顯示在清單查詢結果中之後，才能刪除子網路。

### 刪除子網路中的所有網路介面（如果有的話）
{: #deleting-nics-cli}

實例可以有多個網路介面，那些網路介面可以屬於 VPC 的多個子網路。子網路中的任何網路介面都必須先刪除，才能夠刪除子網路。 

刪除子網路中之網路介面的唯一方法是刪除實例。
{: note}

若要列出帳戶中所有實例，請執行下列指令：

```
ibmcloud is instances
```
{: pre}

如果要刪除 VPC 中的所有實例，您可以對 VPC 中的每個實例執行下列指令，其中 `$vsi` 是要刪除的實例的 ID。刪除實例時，其他子網路中的所有網路介面（如果有的話）會自動刪除。

```
ibmcloud is instance-delete $vsi
```
{: pre}

實例的狀態應該會立即變更為 `deleting`，但該實例仍會顯示在清單查詢結果中。刪除實例可能需要最多 30 分鐘。
{: note}

等待刪除實例時，可以要求平行刪除其他子網路資源。但是，要到子網路中的實例和其他所有資源不再顯示在清單查詢結果中之後，才能刪除子網路。

若要列出實例的所有網路介面，請執行下列指令：

```
ibmcloud is instance-network-interfaces $vsi
```
{: pre}

如果在嘗試刪除的子網路中存在次要網路介面，則您必須刪除實例。在不刪除實例的情況下，無法從實例刪除網路介面。

### 刪除子網路
{: #deleting-subnet-cli}

已刪除子網路內的所有資源後（這意味著清單查詢結果中不會傳回這些資源），請執行下列指令來刪除子網路：

```
ibmcloud is subnet-delete $subnet
```
{: pre}

子網路的狀態應該會立即變更為 `deleting`，但可能需要幾分鐘時間才能實際刪除子網路，隨後它不會再顯示在清單查詢結果中。

## 步驟 3：刪除 VPC 中的所有公用閘道（如果有的話）
{: #deleting-pgs-cli}

若要列出帳戶中所有公用閘道，請執行下列指令：

```
ibmcloud is public-gateways
```
{: pre}

針對 VPC 中您想要刪除的每個公用閘道，請執行下列指令來刪除公用閘道，其中 `$gateway` 是公用閘道 ID。

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


## 步驟 4：刪除 VPC
{: #deleting-single-vpc-cli}

一旦已刪除 VPC 中的所有子網路、VPN 閘道、負載平衡器、公用閘道，請執行下列指令來刪除 VPC，其中 `$vpc` 是您正在刪除之 VPC 的 ID。

```
ibmcloud is vpc-delete $vpc
```
{: pre}

VPC 的狀態應該會立即變更為 `deleting`，但可能需要幾分鐘時間才能實際刪除 VPC，隨後它不會再顯示在清單查詢結果中。
