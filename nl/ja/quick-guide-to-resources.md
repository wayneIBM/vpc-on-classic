---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-17"

keywords: resources, CLI, commands, VPC, subnet, instance, floating, region, endpoint, zone, storage

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}
{:download: .download}

# リソースに対する CLI コマンドのクイック・リファレンス
{: #quick-reference-to-cli-commands-for-resources}

リソースに対する CLI コマンドを記載したこのクイック・ガイドは、初めて {{site.data.keyword.cloud}} CLI を仮想プライベート・クラウドで使用する人の役に立つガイドです。

* **ログインするには**

  * `ibmcloud login [-sso]`

* **VPC についての情報を取得するには**

  * `ibmcloud is vpcs`
  
特定のリソースについての詳細情報を表示するには、リソースの ID を使用します。
{: tip}

* **VPC の詳細情報を表示するには** 

  * `ibmcloud is vpc [vpc_id]` 

* **サブネットについての情報を取得するには** 

  * `ibmcloud is subnets`

* **サブネットについての詳細情報を取得するには**

  * `ibmcloud is subnet [subnet_id]`

* **インスタンスについての情報を取得するには**

  * `ibmcloud is instances
` 

* **インスタンスについての詳細情報を表示するには** 

  * `ibmcloud is instance [instance_id]`

* **浮動 IP についての情報を取得するには** 

  * `ibmcloud is floating-ips`  

* **浮動 IP についての詳細情報を表示するには**

  * `ibmcloud is floating-ip [floating-ips_id]`

* **地域とエンドポイントについての情報を取得するには**

  * `ibmcloud is regions
`

* **ゾーンについての情報を取得するには** 

  * `ibmcloud is zones [region_name]`
  
* **すべてのブロック・ストレージ・ボリュームに関する情報を取得するには**

  * `ibmcloud is volumes`
  
* **ブロック・ストレージ・ボリュームの詳細情報を表示するには**

  * `ibmcloud is volume [volume_id]`
