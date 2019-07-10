---

copyright:
  years: 2019
lastupdated: "2019-06-13"

keywords: release notes, changes, updates, vpc, profile, hyper protect, estimator, load balancer

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

# 发行说明
{: #release-notes}

使用这些发行说明可了解有关 {{site.data.keyword.cloud}} Virtual Private Cloud 最新更改的信息。
{:shortdesc}

## 2019 年 6 月 14 日
{: #june-14-2019}

**对 IBM Cloud VPC UI 的更新**

- **SSH 密钥和资源组**。
    * 资源组会显示在 SSH 密钥列表视图中。
    * 资源组在 SSH 密钥供应模态中可供选择。
- **概要文件和带宽**。分配给每个概要文件的带宽上限会显示在**常用概要文件**和**所有概要文件**显示页面中。
- **资源组**。VSI 供应页面会显示资源组下拉菜单。

**对 Block Storage for VPC 的更新**
- **卷名长度**增加到了 63 个字母数字字符。
- 以下**卷错误代码**已重命名，并且更新或删除了相应消息：
    * `volume_encryption_key_account_id_mismatch` 重命名为 `volume_crn_account_id_mismatch`
    * `volume_encryption_key_cname_mismatch` 重命名为 `volume_crn_cname_mismatch`
    * 除去了 `volume_encryption_key_region_not_found`

**对 SDK 的更新**

- 发布了 **Terraform Provider V0.17.1**。请下载[最新的二进制文件](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1)。
- **Docker 映像**也已使用最新的 Terraform 提供程序进行更新。可以使用以下命令拉取最新的 Docker 映像：`docker pull ibmterraform/terraform-provider-ibm-docker:latest`
- 请务必记住将 `generation` 设置为提供程序自变量，或者导出 `IC_GENERATION= 1`，以便代码使用经典基础架构上当前发布的 VPC 版本。缺省情况下，该值设置为 2（表示即将发布的 VPC 现在处于 Beta 阶段）。
- 提供程序自变量（`bluemix_api_key` 和 `bluemix_timeout`）已不推荐使用，因此在运行 Terraform 套餐或应用 Terraform套餐时可能会抛出警告。

## 2019 年 6 月 7 日
{: #june-07-2019}

- **IBM Cloud VPC 现在已一般可用**。所有[现收现付帐户](/docs/account?topic=account-accounts) 都可以供应 VPC 资源。登录到 [IBM Cloud 控制台](https://{DomainName}/vpc/overview)，立即开始使用吧！

- **现在可以对实例、密钥、卷和安全组设置不同的授权策略**。在 [API 和 CLI 调用所需的资源授权](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls)中了解管理这些资源需要的角色。抢先体验用户不受现有策略的影响。

- **从用户界面、CLI 和 API 中除去了虚拟网络接口的端口速度参数**。

- **现在，用户界面中提供了多区域支持监视度量值**。


## 2019 年 5 月 31 日
{: #may-31-2019}

**新的 API 版本可用**。VPC on Classic API 版本已从 `2019-01-01` 更新为 `2019-05-31`。有关准则和最佳实践，请参阅 Virtual Private Cloud on Classic API 中的[版本控制](https://{DomainName}/apidocs/vpc-on-classic#versioning)。

## 2019 年 5 月 24 日
{: #may-24-2019}

- **现在，处于 deleting 状态的负载均衡器实例会显示在用户界面的列表视图中**。在 UI 中请求负载均衡器后，列表视图会继续显示该负载均衡器，直到删除过程完成为止。

- **现在，用户界面中提供了第 7 层负载均衡器功能**。您现在可以在用户界面中配置第 7 层负载均衡器功能。

- **现在，用户界面中提供了负载均衡器查看和编辑策略**。用户界面中提供了用于查看和编辑负载均衡器策略的新功能。

有关更多信息，请参阅[负载均衡器文档](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)。


## 2019 年 5 月 10 日
{: #may-10-2019}


- **更改了概要文件名称**。实例概要文件名称已更改。需要更新用于供应实例的命令才能使用新的概要文件名称。请在[此处](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles)阅读有关概要文件的更多信息。

- **用户界面中提供了估算的每月浮动 IP 定价**。现在，保留浮动 IP 地址时，可以看到一行显示该浮动 IP 的每月估算价格。

- **Hyper Protect 支持可用**。

- **子网名称和专区显示为链接，可转至用户界面中的关联子网**。现在，子网按名称（而不是按子网标识）列出，并且名称充当关联子网的子网详细信息页面的链接。

- **用户界面中提供了成本摘要估算工具**。

- **对负载均衡器池和侦听器的轮询会反映在用户界面中**。

    * 现在，在负载均衡器池页面的资源表下方，会看到显示上次更新时间的计数器。
    * 缺省轮询时间间隔为 30 秒。
    * 可以显示四 (4) 种不同的状态：`newProvision`、`active`、`failed` 和`其他所有状态`。
        * 对于 newProvision：时间间隔为 15 秒。池和侦听器立即进入活动状态。
        * 对于 active：资源列表和标题每 60 秒更新一次。
        * 对于 failed：轮询已停止。
        * 对于其他所有状态：轮询继续以 30 秒时间间隔执行。
    * 如果删除侦听器，那么会触发标题状态以显示`正在更新（操作不可用）`。
    * 另请注意，执行更新时，会更新标题信息。
    * 这些更改还适用于侦听器轮询。

- **轮询成员时，负载均衡器成员计数显示在用户界面的成员和详细信息页面中**。

    * 在运行状况部分中，可看到成员计数，其旁边是“正在访存实例详细信息”。
    * 在资源表的“实例”列中，可看到成员计数，其旁边是“正在访存实例详细信息”。
    * 成员装入后，如果包含无效的成员，那么您将看到黄色注意图标。

- **用户界面中的 VPC 详细信息页面会显示所有连接的子网**。
