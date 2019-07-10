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

# 指派對 VPC 資源的角色存取權
{: #assigning-role-based-access-to-vpc-resources}

「帳戶管理者」可以利用授權_原則_，以根據個別使用者的_角色_ 來控制對_資源_ 的存取權。也可以針對下列各項套用原則：

(1) 使用者群組的授權（稱為_存取群組_）、

(2) 單一_資源_ 的指派特徵，或者

(3) 資源集合（稱為_資源群組_）。

可以彼此獨立指派資源的授權及使用者的授權。

如需建立使用者、使用者存取群組、資源群組及原則的相關資訊，請參閱[關於資源](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources)。

## 以 IAM 為基礎的存取控制
{: #iam-based-access-control}

一般而言，{{site.data.keyword.cloud}} Virtual Private Cloud 授權及資源管理作法可與 IBM Cloud Identity and Access Management (IAM) 服務合作。如需 IAM、資源群組及存取群組的一般相關資訊，請參閱以下這些 IBM Cloud 文件：

* [IBM Cloud IAM](/docs/iam?topic=iam-getstarted)
* [資源群組](/docs/overview?topic=overview-whatis-rgs)
* [存取群組](/docs/overview?topic=overview-cloudaccess)

已自訂 IBM Cloud IAM Service 的特定特性，以在 IBM Cloud VPC 中使用。[關於資源](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources)文件進一步說明 VPC 中所套用的 IAM 授權原則。

若要彙總，您可以根據以下項目來指派以 IAM 為基礎的授權：

* 個別使用者
* 存取群組（使用者的群組）
* 特定類型的資源
* 資源群組

## 指派使用者許可權
{: #assigning-user-permissions}

對於使用者，透過指派系統定義的 IAM 角色來控制存取權：

* 管理者
* 編輯者
* 操作員
* 檢視者

下表顯示每個使用者角色與其容許用於 VPC 之控制類型間的對應：

| 角色名稱 | 容許的存取類型 |
|-----------|-------------------------|
| 檢視者 | 檢視 VPC、列出 VPC |
| 編輯者 | 檢視 VPC、列出 VPC、建立 VPC、刪除 VPC、更新 VPC |
| 操作員 | 檢視 VPC、列出 VPC |
| 管理者 | 檢視 VPC、列出 VPC、建立 VPC、刪除 VPC、更新 VPC、將原則指派給其他使用者 |


## 後續步驟
{: #permissions-next-steps}

如需為使用者授與以角色為基礎之特定作業許可權的逐步指示，請參閱[管理 VPC 資源的使用者許可權](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)主題。
