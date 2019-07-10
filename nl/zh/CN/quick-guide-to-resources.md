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

# 针对资源的 CLI 命令的快速参考
{: #quick-reference-to-cli-commands-for-resources}

针对资源的 CLI 命令的这一快速指南对于开始使用 {{site.data.keyword.cloud}} CLI for Virtual Private Cloud 非常有用。

* **登录**

  * `ibmcloud login [-sso]`

* **获取有关 VPC 的信息**

  * `ibmcloud is vpcs`
  
使用资源的标识来查看有关资源的特定详细信息。
{: tip}

* **查看 VPC 详细信息** 

  * `ibmcloud is vpc [vpc_id]` 

* **获取有关子网的信息** 

  * `ibmcloud is subnets`

* **获取子网详细信息**

  * `ibmcloud is subnet [subnet_id]`

* **获取有关实例的信息**

  * `ibmcloud is instances` 

* **查看有关实例的详细信息** 

  * `ibmcloud is instance [instance_id]`

* **获取有关浮动 IP 的信息** 

  * `ibmcloud is floating-ips`  

* **查看浮动 IP 详细信息**

  * `ibmcloud is floating-ip [floating-ips_id]`

* **获取有关区域和端点的信息**

  * `ibmcloud is regions`

* **获取有关专区的信息** 

  * `ibmcloud is zones [region_name]`
  
* **获取有关所有块存储卷的信息**

  * `ibmcloud is volumes`
  
* **查看有关块存储卷的详细信息**

  * `ibmcloud is volume [volume_id]`
