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

# 使用 REST API 创建 VPC
{: #deleting-using-api}

使用 REST API 删除 {{site.data.keyword.cloud}} Virtual Private Cloud 时遵循的[删除](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)过程中的常规步骤与使用 [CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli) 进行删除时相同。

下面是删除过程中的主要步骤：

1. 查找要删除的 VPC 中的所有子网
2. 删除 VPC 中的每个子网，这意味着：
    - 删除子网中的所有 VPN 网关。
    - 删除子网中的所有负载均衡器。
    - 删除子网中的所有网络接口（实例）。
    - 删除子网。
3. 删除 VPC 中的所有公共网关。
4. 删除 VPC。

以下各部分提供了一些使用 `curl` 进行的 API 调用示例，可以运行这些调用来删除 VPC。

## 步骤 1：查找要删除的 VPC 中的所有子网
{: #deleting-find-subnets-api}

必须先删除 VPC 中的所有子网，然后才能删除 VPC。通过运行以下命令并查看 `id` 值，获取要删除的 VPC 的标识：

在 curl 命令后添加 ` | json_pp ` 或 ` | jq `，以获取可读的 JSON 字符串。
{: tip}

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

将 VPC 标识保存在变量中，以便稍后可以使用，例如：

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

要获取帐户中所有子网的列表，请运行以下命令：

```bash
curl -X GET "$rias_endpoint/v1/subnets?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

查看 `vpc` 值以确定需要删除的子网。将子网标识保存在变量中，以供稍后使用，例如：

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

如果要删除的 VPC 有多个子网，请重复这些步骤以删除每个子网。
{: important}

## 步骤 2：删除 VPC 中的每个子网
{: #deleting-subnet-resources-api}

### 删除子网中的所有 VPN 网关（如果有）

要列出帐户中的所有 VPN 网关，请运行以下命令：

```bash
curl -X GET "$rias_endpoint/v1/vpn_gateways?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

对于要删除的子网中的每个 VPN 网关，请运行以下命令，其中 `$vpnid` 是 VPN 网关的标识。

```bash
curl -X DELETE "$rias_endpoint/v1/vpn_gateways/$vpnid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

VPN 网关的状态应该立即更改为 `deleting`，但在执行列表查询时仍会显示。删除 VPN 网关最长可能需要 30 分钟。等待删除 VPN 网关时，可以请求并行删除其他子网资源；但是，要到子网中的 VPN 网关和其他所有资源不再显示在列表查询结果中之后，才能删除子网。

### 删除子网中的所有负载均衡器（如果有）
{: #deleting-lbs-api}

要列出帐户中的所有负载均衡器，请运行以下命令：

```bash
curl -X GET "$rias_endpoint/v1/load_balancers?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

对于要删除的子网中的每个负载均衡器，请运行以下命令，其中 `$lbid` 是负载均衡器的标识。

```bash
curl -X DELETE "$rias_endpoint/v1/load_balancers/$lbid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

负载均衡器的状态应该立即更改为 `deleting`，但在执行列表查询时仍会显示。删除负载均衡器最长可能需要 30 分钟。等待删除负载均衡器时，可以请求并行删除其他子网资源；但是，要到子网中的负载均衡器和其他所有资源不再显示在列表查询结果中之后，才能删除子网。

### 删除子网中的所有网络接口（如果有）
{: #deleting-nics-api}

实例可能有多个网络接口，并且这些网络接口可能属于 VPC 的多个子网。删除子网之前，必须先删除子网中的所有网络接口。删除子网中网络接口的唯一方法是删除实例。

要列出帐户中的所有实例，请运行以下命令：

```bash
curl -X GET "$rias_endpoint/v1/instances?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

如果要删除 VPC 中的所有实例，那么可以对 VPC 中的每个实例运行以下命令，其中 `$vsi` 是要删除的实例的标识。删除实例后，将自动删除其他子网中的所有网络接口（如果有）。

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$vsi?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

实例的状态应该立即更改为 `deleting`，但仍可能会显示在列表查询结果中。删除实例最长可能需要 30 分钟。等待删除实例时，可以请求并行删除其他子网资源；但是，要到子网中的实例和其他所有资源不再显示在列表查询结果中之后，才能删除子网。

要列出实例的所有网络接口，请运行以下命令：

```bash
curl -X GET "$rias_endpoint/v1/instances/$vsi/network_interfaces?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

如果在尝试删除的子网中存在辅助网络接口，那么需要删除实例。不能只从实例中删除网络接口，而不删除实例。

### 删除子网
{: #deleting-subnet-api}

删除了子网内的所有资源，并且列表查询结果中不再返回这些资源后，请运行以下命令来删除子网：

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

子网的状态应该立即更改为 `deleting`，但可能需要几分钟时间才能删除子网，删除后其不会再显示在列表查询结果中。

## 步骤 3：删除 VPC 中的所有公共网关（如果有）
{: #deleting-pgs-api}

要列出帐户中的所有公共网关，请运行以下命令：

```bash
curl -X GET "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

对于要删除的 VPC 中的每个公共网关，请运行以下命令来删除公共网关，其中 `$gateway` 是公共网关标识。

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}


## 步骤 4：删除 VPC
{: #deleting-single-vpc-api}

删除了 VPC 中的所有子网、VPN 网关、负载均衡器和公共网关后，运行以下命令来删除 VPC，其中 `$vpc` 是要删除的 VPC 的标识。

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

VPC 的状态应该立即更改为 `deleting`，但可能需要几分钟时间才能删除 VPC，删除后其不会再显示在列表查询结果中。
