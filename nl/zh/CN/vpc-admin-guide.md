---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, control

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

# 分配对 VPC 资源的基于角色的访问权
{: #assigning-role-based-access-to-vpc-resources}

帐户管理员可以利用授权_策略_，这些策略根据单个用户的_角色_来控制对_资源_的访问权。策略还可以应用于：

(1) 对一组用户（称为_访问组_）授权，

(2) 分配给单个_资源_的特征，或者

(3) 资源的集合（称为_资源组_）。

资源授权和用户授权可以彼此独立进行分配。

有关创建用户、用户访问组、资源组和策略的更多信息，请参阅[关于资源](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources)。

## 基于 IAM 的访问控制
{: #iam-based-access-control}

通常，{{site.data.keyword.cloud}} Virtual Private Cloud 授权和资源管理实践可与 IBM Cloud Identity and Access Management (IAM) 服务进行协调。有关 IAM、资源组和访问组的更多一般信息，请参阅以下 IBM Cloud 文档：

* [IBM Cloud IAM](/docs/iam?topic=iam-getstarted)
* [资源组](/docs/overview?topic=overview-whatis-rgs)
* [访问组](/docs/overview?topic=overview-cloudaccess)

IBM Cloud IAM 服务的某些功能已针对在 IBM Cloud VPC 中使用进行了定制。[关于资源](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources)文档说明了有关在 VPC 中应用的 IAM 授权策略的更多信息。

总之，可以基于以下各项分配基于 IAM 的授权：

* 单个用户
* 访问组（用户组）
* 特定类型的资源
* 资源组

## 为用户分配许可权
{: #assigning-user-permissions}

通过为用户分配系统定义的 IAM 角色，可控制其访问权：

* 管理员
* 编辑者
* 操作员
* 查看者

下表显示了每个用户角色与其允许用于 VPC 的控制类型之间的对应关系：

|角色名称|允许的访问类型|
|-----------|-------------------------|
|查看者|查看 VPC，列出 VPC|
|编辑者|查看 VPC，列出 VPC，创建 VPC，删除 VPC，更新 VPC |
|操作员|查看 VPC，列出 VPC|
|管理员|查看 VPC，列出 VPC，创建 VPC，删除 VPC，更新 VPC，为其他用户分配策略|


## 后续步骤
{: #permissions-next-steps}

有关向用户授予基于角色的许可权来完成特定任务的逐步指示信息，请参阅[管理针对 VPC 资源的用户许可权](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)主题。
