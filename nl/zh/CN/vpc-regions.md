---

copyright:
  years: 2019
lastupdated: "2019-05-22"

keywords: region, zone, deploy, datacenter, data, center, federated, CLI, API, account, failover, disaster, recovery, DR

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

# 在不同区域中创建 VPC
{: #creating-a-vpc-in-a-different-region}

区域是一种特定的地理位置，可以在其中部署应用程序、服务和其他 {{site.data.keyword.cloud}} 资源。区域由一个或多个专区组成，专区是物理数据中心，用于容纳主机服务和应用程序的计算、网络和存储资源以及相关冷却系统和电源。专区彼此隔离，可确保在区域内没有共有的单点故障。

IBM Cloud VPC 服务是区域性的。要允许进行灾难恢复 (DR)，您必须通过在其他某个区域中建立 VPC，以提供使用故障转移到备用区域的方式来恢复的能力。
{: note}

Virtual Private Cloud 将分阶段部署到所有 {{site.data.keyword.cloud}} 区域。

|位置|区域|API 端点|状态|
| ------- | :------: | :------: |:------: |
|达拉斯|us-south|`us-south.iaas.cloud.ibm.com`|可用|
|法兰克福|eu-de|`eu-de.iaas.cloud.ibm.com`|可用|
|东京|jp-tok|`jp-tok.iaas.cloud.ibm.com`|可用|
|伦敦|eu-gb|`eu-gb.iaas.cloud.ibm.com`|即将提供|
|悉尼|au-syd|`au-syd.iaas.cloud.ibm.com`|即将提供|
|华盛顿|us-east|`us-east.iaas.cloud.ibm.com`|即将提供|

您登录到特定区域时，IBM Cloud CLI 会自动设置区域 API (VPC) 端点。
{: note}

## 使用 CLI 登录到特定区域
{: #log-in-to-a-specific-region-using-the-cli}

登录到 IBM Cloud 时，可以指定区域，也可以稍后选择区域。例如，要直接登录到达拉斯 (`us-south`) 区域中的全局 API 端点，请根据您是具有联合帐户 (SSO) 还是非联合帐户，运行以下相应命令。

对于联合帐户：

```
ibmcloud login -a https://cloud.ibm.com -r us-south --sso
```
{: pre}

对于非联合帐户：

```
ibmcloud login -a https://cloud.ibm.com -r us-south
```
{: pre}

要稍后选择区域，请不要指定 `-r <region>` 参数，CLI 将提示您选择区域。

示例输出：

```
API endpoint: cloud.ibm.com

Get One Time Code from https://identity-2.eu-central.iam.cloud.ibm.com/identity/passcode to proceed.
Open the URL in the default browser? [Y/n]> y
One Time Code >
Authenticating...
OK

Select an account:
1. MyAccount (00a11aa1a11aa11a1111a1111aaa11aa) <-> 1234567
2. TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321
Enter a number> 2
Targeted account TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321


Targeted resource group Default

Select a region (or press enter to skip):
1. au-syd
2. jp-tok
3. eu-de
4. eu-gb
5. us-south
6. us-east
Enter a number> 5
Targeted region us-south


API endpoint:      https://cloud.ibm.com   
Region:            us-south   
User:              first.last@email.com   
Account:           TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321  
Resource group:    Default   
CF API endpoint:      
Org:                  
Space:                

...
```
{: screen}

## 使用 CLI 切换区域
{: #switch-regions-using-the-cli}

要获取 VPC 区域的最新状态，请运行以下命令：

```
ibmcloud is regions
```
{: pre}

要切换到其他区域，请运行 `ibmcloud target -r <region>` 命令。例如，要切换到法兰克福区域，请运行以下命令：

```
ibmcloud target -r eu-de
```
{: pre}

要检查当前位置，请运行以下命令：

```
ibmcloud target
```
{: pre}

## 使用 API 切换区域  
{: #switch-regions-using-the-api}

要使用 REST 与区域 VPC API 进行交互，请将请求定向到与要在其中创建资源的区域关联的 API 端点。区域的 API 端点如上表所示。此外，通过运行以下命令，可以查找与区域关联的端点：

```
ibmcloud is regions
```
{: pre}


例如，要获取 `us-south` 区域中 VPC 的列表，请运行以下命令：

```
curl "https://us-south.iaas.cloud.ibm.com/v1/vpcs?version=2019-05-31&generation=1" -H "Authorization: $iam_token"
```
{: pre}


## 获取专区
{: #get-zones}

要获取每个区域可用的专区的列表，请运行 `ibmcloud is zones <region>` 命令。例如，要获取 `us-south` 区域中专区的列表，请运行以下命令：

```
ibmcloud is zones us-south
```
{: pre}
