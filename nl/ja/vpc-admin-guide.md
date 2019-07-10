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

# VPC リソースに対する役割ベースのアクセス権限の割り当て
{: #assigning-role-based-access-to-vpc-resources}

アカウント管理者は、権限_ポリシー_ を使用できます。このポリシーでは、個々のユーザーの_役割_ に基づいて、_リソース_ に対するアクセス権限を制御します。 ポリシーは以下にも適用できます。

(1) ユーザー・グループの権限。_アクセス・グループ_ と呼びます。

(2) 単一の_リソース_ に割り当てられた特性。

(3) リソースの集合。_リソース・グループ_ と呼びます。

リソースに対する権限とユーザーに対する権限は、相互に独立して割り当てることができます。

ユーザー、ユーザー・アクセス・グループ、リソース・グループ、ポリシーの作成方法について詳しくは、[リソースについて](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources)を参照してください。

## IAM ベースのアクセス制御
{: #iam-based-access-control}

一般的に、{{site.data.keyword.cloud}} 仮想プライベート・クラウドの権限とリソース管理のための手法では、IBM Cloud の Identity and Access Management (IAM) サービスを利用します。 IAM、リソース・グループ、アクセス・グループの概要について詳しくは、以下の IBM Cloud 資料を参照してください。

* [IBM Cloud IAM](/docs/iam?topic=iam-getstarted)
* [リソース・グループ](/docs/overview?topic=overview-whatis-rgs)
* [アクセス・グループ](/docs/overview?topic=overview-cloudaccess)

IBM Cloud の IAM サービスのいくつかの機能は、IBM Cloud VPC で使用するためにカスタマイズされています。 [リソースについて](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources)の資料で、VPC で適用される IAM 権限ポリシーについて詳細に説明しています。

要約すると、以下に基づいて IAM ベースの権限を割り当てることができます。

* 個々のユーザー
* アクセス・グループ (ユーザーのグループ)
* 特定のタイプのリソース
* リソース・グループ

## ユーザー許可の割り当て
{: #assigning-user-permissions}

ユーザーについては、以下のシステム定義の IAM 役割を割り当てることでアクセス権限を制御します。

* 管理者
* エディター
* オペレーター
* ビューアー

以下の表に、各ユーザー役割と、その役割で許可される VPC の制御のタイプの対応関係を示します。

| 役割名 | 許可されるアクセス権限のタイプ |
|-----------|-------------------------|
| ビューアー | VPC を表示する、VPC をリスト表示する  |
| エディター | VPC を表示する、VPC をリスト表示する、VPC を作成する、VPC を削除する、VPC を更新する |
| オペレーター  | VPC を表示する、VPC をリスト表示する |
| 管理者 |VPC を表示する、VPC をリスト表示する、VPC を作成する、VPC を削除する、VPC を更新する、ポリシーを他のユーザーに割り当てる |


## 次のステップ
{: #permissions-next-steps}

特定の作業のために役割ベースの許可をユーザーに付与する手順を詳しく調べたい場合は、[VPC リソースに対するユーザー許可の管理](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)のトピックを参照してください。
