---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: permissions, infrastructure, VPC, SSH, CLI, API, console, classic

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
{:DomainName: data-hd-keyref="DomainName"}

# 入门教程
{: #getting-started}
[comment]: # (链接的帮助主题)


要开始使用 {{site.data.keyword.cloud}} Virtual Private Cloud，请执行以下操作：

1. 创建虚拟私有云。
2. 在一个或多个专区中的虚拟私有云中创建一个或多个子网。
3. 如果要使子网上的资源有权访问因特网，或使因特网有权访问子网上的资源，请在子网上创建公共网关 (PGW)。
4. 选择要运行的虚拟服务器实例 (VSI) 的概要文件，然后对其进行实例化。
5. 如果要通过因特网访问虚拟服务器实例，请保留浮动 IP 地址，并将其与该实例相关联。
5. 在虚拟服务器实例上部署服务或应用程序。

## 先决条件

 * **用户许可权**：确保您的用户有足够的许可权在 VPC 中创建和管理资源。有关必需许可权的列表，请参阅[授予 VPC 用户所需的许可权](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)。

 * **准备好 SSH 密钥**：您将使用 SSH 密钥来连接到虚拟服务器实例。

   * 在主目录的 `.ssh` 目录下查找名为 `id_rsa.pub` 的文件，例如 `/Users/<USERNAME>/.ssh/id_rsa.pub`。该文件以 `ssh-rsa` 开头，以您的电子邮件地址结尾。

   * 或者生成 SSH 密钥：如果您没有公用 SSH 密钥，或者如果忘记了现有 SSH 密钥的密码，请通过运行 `ssh-keygen` 命令并遵循提示来生成新密钥。例如，可以在 Linux 服务器上，通过运行命令 `ssh-keygen -t rsa -C "user_ID"` 来生成 SSH 密钥。该命令会生成两个文件。生成的公用密钥位于 `<your key>.pub` 文件中。

## 使用 UI、CLI 或 REST API

可以通过 UI、CLI 或 REST API 供应和管理所有 VPC 资源。

如果您对 IBM Cloud Virtual Private Cloud 不熟悉，请选择以下任一链接，这将全程引导您完成创建 IBM Cloud VPC 及其资源的过程。您可以选择是从控制台 UI、命令行 (CLI) 还是从 REST API 开始。

* 要通过用户界面访问，请登录到 [IBM Cloud 控制台 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")]( https://{DomainName}/vpc){: new_window}。有关更多信息，请参阅[使用 IBM Cloud 控制台创建 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-console)。
* 要使用命令行界面，请遵循 [Hello World](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-cli) 示例。
* 对于更高级的用户，可以直接调用 [REST API](https://{DomainName}/apidocs/vpc-on-classic)。请遵循[示例代码](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis)教程以开始使用 REST API。

## 后续步骤
如果您准备好深入探索，请直接转至有关 **VPC 联网**、**VSI for VPC** 和 **Block Storage for VPC** 的详细文档：

* [**Virtual Private Cloud 联网**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-getting-started)
* [**Virtual Servers for Virtual Private Cloud**](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-getting-started)
* [**Block Storage for Virtual Private Cloud**](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)
