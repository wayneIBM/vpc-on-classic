---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, api, rias, ui, console

subcollection: vpc-on-classic

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

# 使用 IBM Cloud 控制台删除 VPC
{: #deleting-using-console}

在 IBM Cloud 控制台中，必须先删除 VPC 的所有公共网关、子网和连接的资源，然后才能删除 VPC。

要使用控制台删除 VPC，请执行以下操作：

1. 查找 VPC 中的所有子网。单击导航窗格中的 **VPC**，然后选择 VPC。 
2. 在 VPC 详细信息页面上，转至**此 VPC 中的子网**列表，然后选择子网以查看其详细信息。
3. 删除连接到子网中的所有资源。在导航窗格中，单击**连接的资源**。对于每个连接的实例、负载均衡器或 VPN 网关，选择相应资源以转至其详细信息页面。在每个资源的详细信息页面上，单击“删除”图标。 

  虽然资源的状态会立即更改为**正在删除**，但完成删除操作可能需要 30 分钟时间。直到控制台中不再显示所有连接的资源后，才能删除子网。
  {: tip}

4. 删除所有连接的资源后，返回到子网的详细信息页面，然后单击“删除”图标。
5. 删除所有子网后，返回到 VPC 的详细信息页面，然后单击“删除”图标。这将删除 VPC 及其所有公共网关。
