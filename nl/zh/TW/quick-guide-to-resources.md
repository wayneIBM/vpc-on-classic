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

# 資源的 CLI 指令快速參照
{: #quick-reference-to-cli-commands-for-resources}

這本用於資源的 CLI 指令快速手冊有助於使用 {{site.data.keyword.cloud}} CLI for Virtual Private Cloud。

* **登入**

  * `ibmcloud login [-sso]`

* **取得 VPC 的相關資訊**

  * `ibmcloud is vpcs`
  
使用資源的 ID 來檢視資源的特定詳細資料。
{: tip}

* **檢視 VPC 詳細資料** 

  * `ibmcloud is vpc [vpc_id]` 

* **取得子網路的相關資訊** 

  * `ibmcloud is subnets`

* **取得子網路詳細資料**

  * `ibmcloud is subnet [subnet_id]`

* **取得實例的相關資訊**

  * `ibmcloud is instances` 

* **檢視實例的詳細資料** 

  * `ibmcloud is instance [instance_id]`

* **取得浮動 IP 的相關資訊** 

  * `ibmcloud is floating-ips`  

* **檢視浮動 IP 詳細資料**

  * `ibmcloud is floating-ip [floating-ips_id]`

* **取得地區及端點的相關資訊**

  * `ibmcloud is regions`

* **取得區域的相關資訊** 

  * `ibmcloud is zones [region_name]`
  
* **取得有關所有區塊儲存空間磁區的資訊**

  * `ibmcloud is volumes`
  
* **檢視有關區塊儲存空間磁區的詳細資料**

  * `ibmcloud is volume [volume_id]`
