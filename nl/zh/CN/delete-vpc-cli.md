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

# 使用 IBM Cloud CLI 删除 VPC
{: #deleting-using-cli}

本主题提供了有关对于每种 VPC 资源，如何按建议的顺序从 {{site.data.keyword.cloud}} Virtual Private Cloud 中删除资源的示例。 

下面是要务必记住的有关删除的一些关键事实：

* 在 VPC 为空之前，无法删除 VPC。 
* 必须先成功删除父资源包含的所有资源，然后才能删除父资源。 
* 如果资源处于暂挂删除状态，那么在列表查询中查看时，该资源会显示 `deleting` 状态。 
* 大多数删除请求是_异步_执行的，这意味着资源尚未完全删除时，可能会在列表查询中显示 **deleting** 状态。 
* 尝试删除父资源之前，必须轮询以确保子资源不再显示在列表视图中。 

资源的状态从 `deleting` 更改为 `failed` 后，可以重试删除该资源。如果无法删除处于 `failed` 状态的资源，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。
{: tip}

## 步骤 1：查找要删除的 VPC 中的所有子网
{: #deleting-find-subnets-cli}

必须先删除 VPC 中的所有子网，然后才能删除 VPC。通过运行以下命令并查看 `ID` 值，获取要删除的 VPC 的标识：

```
ibmcloud is vpcs
```
{: pre}

将 VPC 标识保存在变量中，以便稍后可以使用，例如：

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

要获取帐户中所有子网的列表，请运行以下命令：

```
ibmcloud is subnets
```
{: pre}

查看 `VPC` 值以确定需要删除的子网。将子网标识保存在变量中，以便稍后可以使用，例如：

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

如果要删除的 VPC 有多个子网，请重复这些步骤以删除每个子网。
{: important}

## 步骤 2：删除 VPC 中的每个子网
{: #deleting-subnet-resources-cli}

### 删除子网中的所有 VPN 网关（如果有）

要列出帐户中的所有 VPN 网关，请运行以下命令：

```
ibmcloud is vpn-gateways
```
{: pre}

对于要删除的子网中的每个 VPN 网关，请运行以下命令，其中 `$vpnid` 是 VPN 网关的标识。

```
ibmcloud is vpn-gateway-delete $vpnid
```

VPN 网关的状态应该立即更改为 `deleting`，但仍会显示在列表查询结果中。删除 VPN 网关最长可能需要 30 分钟。
{: note}

等待删除 VPN 网关时，可以请求并行删除其他子网资源。但是，要到子网中的 VPN 网关和其他所有资源不再显示在列表查询结果中之后，才能删除子网。

### 删除子网中的所有负载均衡器（如果有）
{: #deleting-lbs-cli}

要列出帐户中的所有负载均衡器，请运行以下命令：

```
ibmcloud is load-balancers
```
{: pre}

对于要删除的子网中的每个负载均衡器，请运行以下命令，其中 `$lbid` 是负载均衡器的标识。

```
ibmcloud is load-balancer-delete $lbid
```
{: pre}

负载均衡器的状态应该立即更改为 `deleting`，但仍会显示在列表查询结果中。删除负载均衡器最长可能需要 30 分钟。
{: note}

等待删除负载均衡器时，可以请求并行删除其他子网资源。但是，要到子网中的负载均衡器和其他所有资源不再显示在列表查询结果中之后，才能删除子网。

### 删除子网中的所有网络接口（如果有）
{: #deleting-nics-cli}

实例可能有多个网络接口，并且这些网络接口可能属于 VPC 的多个子网。删除子网之前，必须先删除子网中的所有网络接口。 

删除子网中网络接口的唯一方法是删除实例。
{: note}

要列出帐户中的所有实例，请运行以下命令：

```
ibmcloud is instances
```
{: pre}

如果要删除 VPC 中的所有实例，那么可以对 VPC 中的每个实例运行以下命令，其中 `$vsi` 是要删除的实例的标识。删除实例后，将自动删除其他子网中的所有网络接口（如果有）。

```
ibmcloud is instance-delete $vsi
```
{: pre}

实例的状态应该立即更改为 `deleting`，但仍会显示在列表查询结果中。删除实例最长可能需要 30 分钟。
{: note}

等待删除实例时，可以请求并行删除其他子网资源。但是，要到子网中的实例和其他所有资源不再显示在列表查询结果中之后，才能删除子网。

要列出实例的所有网络接口，请运行以下命令：

```
ibmcloud is instance-network-interfaces $vsi
```
{: pre}

如果在尝试删除的子网中存在辅助网络接口，那么必须删除实例。不能只从实例中删除网络接口，而不删除实例。

### 删除子网
{: #deleting-subnet-cli}

删除了子网内的所有资源后（这意味着列表查询结果中不会返回这些资源），请运行以下命令来删除子网：

```
ibmcloud is subnet-delete $subnet
```
{: pre}

子网的状态应该立即更改为 `deleting`，但可能需要几分钟时间才能实际删除子网，随后其不会再显示在列表查询结果中。

## 步骤 3：删除 VPC 中的所有公共网关（如果有）
{: #deleting-pgs-cli}

要列出帐户中的所有公共网关，请运行以下命令：

```
ibmcloud is public-gateways
```
{: pre}

对于要删除的 VPC 中的每个公共网关，请运行以下命令来删除公共网关，其中 `$gateway` 是公共网关标识。

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


## 步骤 4：删除 VPC
{: #deleting-single-vpc-cli}

删除了 VPC 中的所有子网、VPN 网关、负载均衡器和公共网关后，运行以下命令来删除 VPC，其中 `$vpc` 是要删除的 VPC 的标识。

```
ibmcloud is vpc-delete $vpc
```
{: pre}

VPC 的状态应该立即更改为 `deleting`，但可能需要几分钟时间才能实际删除 VPC，随后其不会再显示在列表查询结果中。
