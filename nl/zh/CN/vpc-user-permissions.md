---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, team, scenario, manage, create, IAM

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

# 管理 VPC 资源的用户许可权
{: #managing-user-permissions-for-vpc-resources}

{{site.data.keyword.cloud}} Virtual Private Cloud 使用基于角色的访问控制，支持帐户管理员控制其用户对 VPC 资源的访问权。

有关 IBM Cloud VPC 如何使用基于角色的访问控制以及 VPC 如何使用 IAM 角色和访问策略的更多信息，请参阅[分配对 VPC 资源的基于角色的访问权](/docs/vpc-on-classic?topic=vpc-on-classic-assigning-role-based-access-to-vpc-resources)。

本文档说明了帐户管理员如何向帐户添加更多用户，并为这些用户提供管理 [VPC 基础架构资源](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources)的正确许可权。文档中涵盖了针对 VPC 管理员的两个常见场景：

* **简单访问场景：**说明如何为用户分配访问策略，以便用户可以创建和使用 VPC 基础架构资源（包括虚拟私有云）。

* **团队访问场景：**说明如何设置资源组和访问策略，以允许两个单独的团队创建和使用分配给其团队的 VPC 资源。

对 VPC 的 IAM 访问策略的更改可能最长需要 10 分钟才能生效。
{: note}

## 简单访问场景
{: #simple-access-scenario}

此场景涵盖设置单个用户所需的基本步骤。我们将包含两个用例：邀请新用户加入帐户和修改现有用户的许可权。

### 邀请新用户创建或管理 VPC 资源
{: #inviting-a-new-user-to-create-or-manage-vpc-resources}

邀请 IBM Cloud 用户加入您的帐户，并为他们提供对 `VPC 基础架构`的访问权，以便他们有权查看、创建和更新 VPC 资源。此部分提供了 IAM 步骤速览。通过接近此文档末尾的**相关链接**部分中的链接，可以获取进一步的信息。

下面是在 IAM 中邀请用户加入 VPC 服务和资源所需执行的基本步骤：

1. 导航至 IBM Cloud 控制台中的 [IAM 用户 UI ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/iam#/users){: new_window}。
2. 在**用户**页面上，单击**邀请用户**。
3. 在**邀请用户**页面上**用户**部分中的**电子邮件地址**字段中，输入要邀请的用户的电子邮件地址。
4. 在**访问权**部分中，展开**服务**，然后完成以下任务：
  * 从**分配对以下对象的访问权**列表中，选择**资源**。
  * 从**服务**列表中，选择 **VPC 基础架构**。
  * 选择要分配给用户的平台访问角色。可以是**管理员**、**编辑者**、**操作员**或**查看者**。
  * 单击**邀请用户**。

### 向现有用户授予管理 VPC 资源的许可权
{: #giving-an-existing-user-permission-to-manage-vpc-resources}

此场景涵盖向帐户中的现有用户授予编辑 VPC 资源的许可权所需执行的基本步骤。

在后续步骤中，您将创建两个 IAM 策略。需要这两个策略，用户才能创建和使用 VPC 基础架构资源。所有资源都必须位于帐户的缺省资源组中。

1. 导航至 IBM Cloud 控制台中的 [IAM 用户 UI ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/iam#/users){: new_window}。
2. 选择要启用其授权的用户。
3. 在**访问策略**选项卡下，选择**分配访问权**。
4. 选择**在资源组中分配访问权**。
5. 选择帐户的缺省资源组。
6. 确保**分配对资源组的访问权**选项保持设置为**查看者**。
7. 要分配正确的服务，请选择 **VPC 基础架构**。
8. 确保**资源类型**值设置为**所有资源类型**。
9. 选择**编辑者**角色。
10. 单击**分配**。

现在，用户有权在帐户的缺省资源组中创建和使用 VPC 资源。第一个策略（步骤 1-6）允许用户查看帐户的缺省资源组。第二个策略（步骤 7-10）为用户分配了对 VPC 基础架构资源的**编辑者**角色，但仅限于访问帐户的缺省资源组中的资源。

{: tip}

## 查看用户的许可权
{: #viewing-your-user-s-permissions}

策略可以在用户的**访问策略**选项卡中进行查看。

可以使用以下 CLI 命令来验证通过策略或访问组分配给用户的资源组许可权：

**通过策略**
```
ibmcloud iam user-policies <username>
```
**通过访问组**
```
ibmcloud iam access-groups -u <username>
```

对 VPC 的 IAM 访问策略的更改可能最长需要 10 分钟才能生效。
{: note}

## 团队访问场景
{: #team-access-scenario}

此场景涵盖帐户管理员可以如何分配授权，以便不同团队有权访问不同的 VPC 资源。此示例使用_资源组_为两个团队设置不同的资源访问权。对于此示例的用途，资源不会跨团队共享。

此示例将引导您逐步创建资源组、创建访问组以及分配相应策略以向团队提供对不同 VPC 资源的访问权。

假设要设置两个不同的项目团队来使用两个不同的虚拟私有云。您将为团队成员分配访问权，以便每个团队都有权访问其团队的 VPC 资源（仅限这些资源）。

* 第一个团队是测试团队。您已决定为该团队分配对名为 `test_vpcs` 的资源组中 VPC 的访问权。
* 第二个团队是生产团队。将为该团队分配对名为 `production_vpcs` 的资源组中 VPC 的访问权。

此策略可用于将不同的 VPC 资源分配给任意数量的团队。但是，所有资源都共享帐户的相同 VPC 配额。有关配额和限制的更多信息，请参阅 [VPC 配额](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#quotas)。
{: tip}

### 步骤 1：创建资源组
{: #step-1-create-resource-groups}

缺省情况下，帐户管理员有权创建新的资源组。首先，必须为其他用户分配对**所有帐户管理服务**的**编辑者**角色，这允许这些用户创建资源组。

第一个任务是创建资源组以包含每个团队的 VPC 资源。

1. 创建名为 `test_team` 的资源组。  
2. 创建名为 `production_team` 的资源组。  

有关如何创建资源组的更多信息，请参阅[管理资源组](/docs/resources?topic=resources-rgs)。

### 步骤 2：创建访问组
{: #step-2-create-access-groups}

资源访问权可以分配给单个用户，也可以分配给用户组。具有相同访问许可权的用户的组称为_访问组_。在此场景中，将创建一个访问组，用于代表需要特定类型 VPC 访问权的团队成员的每个分组，共有 4 个唯一访问组。

**任务：**创建具有以下名称的 4 个访问组：`test_team_manage_vpcs`、`test_team_view_vpcs`、`production_team_manage_vpcs` 和 `production_team_view_vpcs`。

有关创建访问组的帮助，请参阅[创建访问组](/docs/iam?topic=iam-groups#create_ag)。

### 步骤 3：向刚才创建的访问组添加 IAM 策略
{: #step-3-add-iam-policies-to-the-access-groups-you-just-created}

添加必需的 VPC 访问策略，以便 `test_team` 访问组可以管理（创建、更新和删除）VPC 资源。  

1. 导航至 IBM Cloud 控制台中的 [IAM 组 UI ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/iam#/groups){: new_window}。
2. 选择所需的访问组（首先选择 `test_team_manage_vpcs`）。
3. 在**访问策略**选项卡下，单击**分配访问权**。
4. 选择**在资源组中分配访问权**。
5. 选择所需的资源组（首先选择 `test_team`）。
6. 确保**分配对资源组的访问权**保持设置为**查看者**。
7. 对于服务，选择 **VPC 基础架构**。
8. 确保**资源类型**保持设置为**所有资源类型**。
9. 选择**编辑者**角色。
10. 选择**分配**。

这些步骤还会分配对 `test_team` 资源组的**查看者**访问权。需要查看者访问权才能更新、创建和删除 `test_team` 资源组内的资源。

{: tip}

对于其余 3 个访问组，重复上述步骤。通过完成这些任务，可将访问组与资源组相匹配：

* 为 `test_team_view_vpcs` 访问组分配对 `test_team` 资源组中 VPC 基础架构资源的**查看者**角色。
* 为 `production_team_manage_vpcs` 访问组分配对 `production_team` 资源组中 VPC 基础架构资源的**编辑者**角色。
* 为 `production_team_view_vpcs` 访问组分配对 `production_team` 资源组中 VPC 基础架构资源的**查看者**角色。

具有对 VPC 资源的**编辑者**访问权的用户还可以查看这些资源。无需将成员添加到**编辑者**和**查看者**访问组。

#### 设置查看者访问权
{: #setting-up-viewer-access)

VPC 基础架构`浮动 IP` 资源在帐户的缺省资源组中创建。因此，需要管理`浮动 IP` 的用户需要对帐户缺省资源组的**查看者**访问权。
{: tip}

下面是如何创建该**查看者**访问权的步骤：

1. 导航至 IBM Cloud 控制台中的 [IAM 组 UI ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/iam#/groups){: new_window}。
2. 选择所需的访问组（首先选择 `test_team_manage_vpcs`）。
3. 在**访问策略**选项卡下，单击**分配访问权**。
4. 选择**分配对帐户管理服务的访问权**。
7. 对于服务，选择**所有帐户管理服务**。
9. 选择**查看者**角色。
10. 选择**分配**。

重复上述步骤，为 `production_team_manage_vpcs` 访问组分配对**所有帐户管理服务**的**查看者**角色。

### 步骤 4：向访问组添加用户
{: #step-4-add-users-to-the-access-groups}

现在，可以将团队成员（用户）添加到相应的访问组。执行以下步骤，将测试团队的每个成员添加到允许测试团队 VPC 管理的访问组：

1. 导航至 IBM Cloud 控制台中的 [IAM 组 UI ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/iam#/groups){: new_window}。
2. 选择所需的访问组（首先选择 `test_team_manage_vpcs`）。
3. 在**用户**选项卡下，单击**添加用户**。
4. 选择要添加到访问组的每个用户。
5. 单击**添加到组**。

重复上述步骤以完成以下任务：
* 将所需的用户分配给 `test_team_view_vpcs` 访问组。
* 将所需的用户分配给 `production_team_manage_vpcs` 访问组。
* 将所需的用户分配给 `production_team_view_vpcs` 访问组。

如果用户具有管理访问权，那么不必为这些用户分配对 VPC 资源的**查看者**访问权。
{: tip}

## 后续步骤
{: #permissions-next-steps}

现在，两个示例团队已设置妥当，可开始使用 VPC。此时，`test_team_manage_vpcs` 和 `production_team_manage_vpcs` 访问组的成员可以在其分配的资源组中创建 VPC。


## 相关链接
{: #permissions-related-links}

* [管理身份和访问权](/docs/iam?topic=iam-getstarted)
* [管理用户和访问权](/docs/iam?topic=iam-iamuserinv)
* [什么是 IAM](/docs/iam?topic=iam-iamoverview)
