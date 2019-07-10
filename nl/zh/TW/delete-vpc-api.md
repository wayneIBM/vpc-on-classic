---

copyright:
  years: 2019
lastupdated: "2019-05-07"


keywords: vpc, delete, resources, api, rias

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# 使用 REST API 刪除 VPC
{: #deleting-using-api}

使用 REST API 刪除 {{site.data.keyword.cloud}} Virtual Private Cloud 時遵循的[刪除](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)程序中的一般步驟與使用 [CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli) 進行刪除時相同。

下面是刪除程序中的主要步驟：

1. 尋找要刪除的 VPC 中的所有子網路。
2. 刪除 VPC 中的每個子網路，這意味著：
    - 刪除子網路中的所有 VPN 閘道。
    - 刪除子網路中的所有負載平衡器。
    - 刪除子網路中的所有網路介面（實例）。
    - 刪除子網路。
3. 刪除 VPC 中的所有公用閘道。
4. 刪除 VPC。

下列各區段提供了一些使用 `curl` 進行的 API 呼叫範例，可以執行這些呼叫來刪除 VPC。

## 步驟 1：尋找要刪除的 VPC 中的所有子網路
{: #deleting-find-subnets-api}

VPC 的每個子網路都必須先刪除，才能刪除 VPC。透過執行下列指令並查看 `id` 值，取得要刪除的 VPC 的 ID：

在 curl 指令後新增 ` | json_pp ` 或 ` | jq `，以取得可讀的 JSON 字串。
{: tip}

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

將 VPC 的 ID 儲存在變數中，以供稍後使用，例如：

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

若要取得帳戶中所有子網路的清單，請執行下列指令：

```bash
curl -X GET "$rias_endpoint/v1/subnets?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

查看 `vpc` 值以確定需要刪除的子網路。將子網路 ID 儲存在變數中，以供稍後使用，例如：

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

如果要刪除的 VPC 有多個子網路，請重複這些步驟以刪除每個子網路。
{: important}

## 步驟 2：刪除 VPC 中的每個子網路
{: #deleting-subnet-resources-api}

### 刪除子網路中的所有 VPN 閘道（如果有的話）

若要列出帳戶中所有 VPN 閘道，請執行下列指令：

```bash
curl -X GET "$rias_endpoint/v1/vpn_gateways?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

針對子網路中您要刪除的每個 VPN 閘道，請執行下列指令，其中 `$vpnid` 是 VPN 閘道的 ID。

```bash
curl -X DELETE "$rias_endpoint/v1/vpn_gateways/$vpnid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

VPN 閘道的狀態應該會立即變更為 `deleting`，但在執行清單查詢時仍會傳回該閘道。刪除 VPN 閘道可能需要最多 30 分鐘。等待刪除 VPN 閘道時，可以要求平行刪除其他子網路資源；但是，要到子網路中的 VPN 閘道和其他所有資源不再顯示在清單查詢結果中之後，才能刪除子網路。

### 刪除子網路中的所有負載平衡器（如果有的話）
{: #deleting-lbs-api}

若要列出帳戶中所有負載平衡器，請執行下列指令：

```bash
curl -X GET "$rias_endpoint/v1/load_balancers?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

針對子網路中您要刪除的每個負載平衡器，請執行下列指令，其中 `$lbid` 是負載平衡器的 ID。

```bash
curl -X DELETE "$rias_endpoint/v1/load_balancers/$lbid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

負載平衡器的狀態應該會立即變更為 `deleting`，但在執行清單查詢時仍會傳回該負載平衡器。刪除負載平衡器可能需要最多 30 分鐘。等待刪除負載平衡器時，可以要求平行刪除其他子網路資源；但是，要到子網路中的負載平衡器和其他所有資源不再顯示在清單查詢結果中之後，才能刪除子網路。

### 刪除子網路中的所有網路介面（如果有的話）
{: #deleting-nics-api}

實例可以有多個網路介面，那些網路介面可以屬於 VPC 的多個子網路。刪除子網路之前，必須先刪除該子網路中的任何網路介面。刪除子網路中之網路介面的唯一方法是刪除實例。

若要列出帳戶中所有實例，請執行下列指令：

```bash
curl -X GET "$rias_endpoint/v1/instances?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

如果要刪除 VPC 中的所有實例，您可以對 VPC 中的每個實例執行下列指令，其中 `$vsi` 是要刪除的實例的 ID。刪除實例時，其他子網路中的所有網路介面（如果有的話）會自動刪除。

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$vsi?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

實例的狀態應該會立即變更為 `deleting`，但該實例仍可能會顯示在清單查詢結果中。刪除實例可能需要最多 30 分鐘。等待刪除實例時，可以要求平行刪除其他子網路資源；但是，要到子網路中的實例和其他所有資源不再顯示在清單查詢結果中之後，才能刪除子網路。

若要列出實例的所有網路介面，請執行下列指令：

```bash
curl -X GET "$rias_endpoint/v1/instances/$vsi/network_interfaces?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

如果在您嘗試刪除的子網路中有次要網路介面存在，則您需要刪除實例。在不刪除實例的情況下，無法從實例刪除網路介面。

### 刪除子網路
{: #deleting-subnet-api}

已刪除子網路內的所有資源，並且執行清單查詢時不再傳回這些資源後，請執行下列指令來刪除子網路：

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

子網路的狀態應該會立即變更為 `deleting`，但可能需要幾分鐘時間才能刪除子網路，刪除後它不會再在清單查詢結果中傳回。

## 步驟 3：刪除 VPC 中的所有公用閘道（如果有的話）
{: #deleting-pgs-api}

若要列出帳戶中所有公用閘道，請執行下列指令：

```bash
curl -X GET "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

針對 VPC 中您想要刪除的每個公用閘道，請執行下列指令來刪除公用閘道，其中 `$gateway` 是公用閘道 ID。

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}


## 步驟 4：刪除 VPC
{: #deleting-single-vpc-api}

一旦已刪除 VPC 中的所有子網路、VPN 閘道、負載平衡器、公用閘道，請執行下列指令來刪除 VPC，其中 `$vpc` 是您正在刪除之 VPC 的 ID。

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

VPC 的狀態應該會立即變更為 `deleting`，但可能需要幾分鐘時間才能刪除 VPC，刪除後它不會再顯示在清單查詢結果中。
