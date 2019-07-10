---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: troubleshoot, tips, limitations, debug, mode, error, bearer, token, API, CLI, endpoint, problem, reboot, 409, status, instance, reset, asynchronous

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
{:troubleshoot: .troubleshoot}

# 有关 IBM Cloud VPC 的故障诊断
{: #troubleshooting-your-ibm-cloud-vpc}

本文档涵盖您可能会遇到的常见问题，并提供了一些有用的提示。

## 使用 CLI 时开启 `TRACE` 方式
{: #troubleshoot}

要在使用 CLI 时开启 `TRACE`（调试）方式，请在 CLI 命令前面设置 `IBMCLOUD_TRACE=true`。

例如：

 ```
IBMCLOUD_TRACE=true ibmcloud is pubgws
```

## 使用 API 时采用 DEBUG 方式或 `verbose` 方式
{: #troubleshoot}

例如，可以向 `curl` 命令添加 `--verbose`，然后向我们发送返回的 `X-Request-Id:` 值，以便我们可以对问题进行故障诊断。遇到连接问题时，`--verbose` 标志特别有用。

另一个选项是向 `curl` 命令添加 `-i` 标志，以便支持团队可以在您请求的响应中看到头。您需要联系支持人员时，此标志在大多数情况下非常有用。

## API 调用需要不记名令牌
{: #troubleshoot}

通过 cURL 命令使用 API 时，可能需要在 Authorization 头中包含“Bearer”，具体取决于 `$iam_token` 中的内容。如果 `$iam_token` 中包含“Bearer”这个词，那么在 Authorization 头中不用重复包含该词。我们的大部分示例假定“Bearer”包含在头中。

## 常见问题
{: #troubleshoot}

下面是您可能会遇到的一些问题。

### 未获授权（401 或 403 错误）

{: #troubleshoot}

您的帐户可能无权使用 VPC。请确保使用的是已加载的帐户。

### 无法创建资源
{: #troubleshoot}

如果您无法创建 VPC 或其他资源，请确保帐户所有者已向您授予正确的[许可权](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)。

### 实例全部处于未知状态
{: #troubleshoot}

确保帐户所有者已向您授予查看和管理实例的正确[许可权](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)。

### API 无响应
{: #troubleshoot}

如果 API 不再返回任何 JSON，那么可能是 IAM 令牌已到期，需要刷新。请重新登录到 IBM Cloud，或者通过运行 `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')` 来刷新令牌。

### 错误：对实例调用操作时，发生 409 冲突
{: #troubleshoot}

如果实例的状态与操作相冲突，那么无法调用该实例的特定操作。例如，如果实例状态为“已停止”，而您尝试运行 `reboot` 操作，那么系统会返回 409 错误。

| 状态        | 操作       | 冲突      |
| ----------- | ---------- | --------  |
| 正在运行    | start      | 是        |
| 已停止      | 除 start 外的任何操作  | 是     |
| 未在运行    | pause      | 是        |
| 未在运行    | reboot     | 是        |
| 未暂停      | resume     | 是        |
| 已暂停      | 除 resume 外的任何操作 | 是     |


### 实例未响应 `instance-reboot` 请求
{: #troubleshoot}

如果实例未响应 `instance-reboot` 请求，那么可以尝试 `instance-reset` 请求。您可以将 `instance-reboot` 视为软请求，而将 `instance-reset` 视为硬请求。`instance-reboot` 请求是向实例发送操作系统重新引导请求，而 `instance-reset` 请求是对 VSI 实例执行硬重置。您可以将这两者的差别视为是在计算机键盘上输入“ctrl-alt-delete”与按重置或电源按钮的差别。务必记住的是，完成 `instance-reset` 请求需要的时间比 `instance-reboot` 请求长。


### 无法删除资源
{: #troubleshoot}

某些操作（例如，创建和删除 VSI 以及创建和删除子网）是通过 API 异步完成的。因此，建议您先轮询要删除的资源，以检查删除情况，然后再继续。

由于这些异步操作，从系统中删除资源可能需要几分钟时间。为了便于删除，最佳做法是按以下顺序执行删除操作：

1. 删除实例
2. 删除公共网关
3. 删除子网
4. 删除 VPC
有关具体信息，请参阅有关[如何删除 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) 的指示信息。
