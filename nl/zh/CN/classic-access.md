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

# 设置 VPC 对经典基础架构的访问权
{: #setting-up-access-to-your-classic-infrastructure-from-vpc}
[comment]: # (链接的帮助主题)

对于任何帐户，您可以在每个区域中设置一个 VPC 来访问 {{site.data.keyword.cloud}} 经典基础架构（包括 Direct Link 连接）。这些特殊的“经典访问 VPC”使用的功能（隐式路由器）与 {{site.data.keyword.cloud}} 经典基础架构的相同。读者应该已经熟悉经典基础架构联网。

设置 VPC 进行经典访问时，经典帐户中没有公共接口的每个计算主机（VSI 或裸机）都可以与经典访问 VPC 之间进行收发包操作。但是，请记住，防火墙、网关、网络 ACL 或安全组可以过滤此流量。作为最佳实践，建议您仅允许应用程序正常运行所需的流量。

在具有公共接口的主机中，必须将路径添加回启用经典的 VPC。
{: important}

## 先决条件：
{: #vpc-prerequisites}

1. 经典帐户必须已链接到 IBM Cloud 帐户。请参阅[链接 IBM 标识帐户](/docs/account?topic=account-unifyingaccounts)，以获取有关如何执行此操作的指示信息。
1. 必须对经典帐户启用 VRF。
    * 如果帐户上已具有 Direct Link，说明您已准备就绪。
    * 如果帐户未启用 VRF，请开具凭单，以便为帐户请求“VRF 迁移”。请参阅 Direct Link 文档中的[如何启动转换](/docs/infrastructure/direct-link?topic=direct-link-how-you-can-initiate-the-conversion#how-you-can-initiate-the-conversion)，以了解有关转换过程的更多信息。

防火墙、网关、网络 ACL 或安全组可以过滤掉经典基础架构与 VPC 资源之间的部分或全部网络流量。
{: important}

使用经典访问的 VPC 中的所有子网都将共享到经典基础架构 VRF 中，该 VRF 使用 `10.0.0.0/8` 空间中的 IP 地址。为了避免 IP 地址冲突，在经典访问 VPC 中创建子网时，**不要使用** `10.0.0.0/8` 空间上的 IP 地址。
{: important}

## 创建经典访问 VPC
{: #create-a-classic-access-vpc}

可以使用用户界面 (UI)、命令行界面 (CLI) 或应用程序编程接口 (API) 来创建具有“经典访问”功能的 VPC。

创建 VPC 时，必须将其设置用于“经典访问”：无法通过更新 VPC 来添加或除去“经典访问”功能。
{: note}

### 通过用户界面创建经典访问 VPC
{: #create-a-classic-access-vpc-from-the-user-interface}

在**创建 VPC** 页面中，通过单击**经典访问**标签下名为**启用对经典资源的访问权**的复选框来创建经典访问 VPC。

![classic-access-ui](/images/classic-access-ui.png)

### 使用 CLI 创建经典访问 VPC
{: #create-a-classic-access-vpc-using-the-cli}

创建 VPC 时，请使用 `--classic-access` 标志。例如：

```
ibmcloud is vpc-create my-access-vpc --classic-access
```
{: pre}


### 使用 API 创建经典访问 VPC
{: #create-a-classic-access-vpc-using-the-api}

创建 VPC 时，请传入 `classic_access` 参数。例如：

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=2019-05-31&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### 经典访问 VPC 缺省地址前缀
{: #classic-access-default-address-prefixes}

使用经典访问的 VPC 中的所有子网都将共享到经典基础架构 VRF 中，该 VRF 使用 `10.0.0.0/8` 空间中的 IP 地址。为了避免 IP 地址冲突，在经典访问 VPC 中创建子网时，**不要使用** `10.0.0.0/8` 空间上的 IP 地址。
{: important}

创建新的经典访问 VPC 时，会根据区域和专区来分配缺省地址前缀，如下表中所示：

专区 (Zone)|地址前缀
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

* 只有专用（在旧文档中称为“后端”）网络才会连接到帐户的专用隐式路由器。
* 只有使用经典 API 分配的子网才会连接到专用隐式路由器的经典端。隐式路由器的路由功能在经典 VLAN 上的子网之间不起作用。
* 每个帐户每个区域只能有一个 VPC 启用经典访问。

根据您是在经典 VSI 还是在裸机服务器上安装的操作系统，配置过程将有所不同。有关更多信息，请参阅[关于公共虚拟服务器](https://cloud.ibm.com/docs/vsi?topic=virtual-servers-about-public-virtual-servers)。
{: note}
