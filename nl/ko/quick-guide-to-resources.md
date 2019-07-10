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

# 리소스 관련 CLI 명령에 대한 빠른 참조
{: #quick-reference-to-cli-commands-for-resources}

리소스 관련 CLI 명령에 대한 이 빠른 안내서는 Virtual Private Cloud에 대한 {{site.data.keyword.cloud}} CLI를 시작하는 데 유용합니다.

* **로그인하려는 경우**

  * `ibmcloud login [-sso]`

* **VPC에 대한 정보를 얻으려는 경우**

  * `ibmcloud is vpcs`
  
리소스에 대한 특정 세부사항을 보려면 리소스 ID를 사용하십시오.
{: tip}

* **VPC 세부사항을 보려는 경우** 

  * `ibmcloud is vpc [vpc_id]` 

* **서브넷에 대한 정보를 얻으려는 경우** 

  * `ibmcloud is subnets`

* **서브넷 세부사항을 얻으려는 경우**

  * `ibmcloud is subnet [subnet_id]`

* **인스턴스에 대한 정보를 얻으려는 경우**

  * `ibmcloud is instances` 

* **인스턴스에 대한 세부사항을 보려는 경우** 

  * `ibmcloud is instance [instance_id]`

* **유동 IP에 대한 정보를 얻으려는 경우** 

  * `ibmcloud is floating-ips`  

* **유동 IP 세부사항을 보려는 경우**

  * `ibmcloud is floating-ip [floating-ips_id]`

* **지역 및 엔드포인트에 대한 정보를 얻으려는 경우**

  * `ibmcloud is regions`

* **구역에 대한 정보를 얻으려는 경우** 

  * `ibmcloud is zones [region_name]`
  
* **모든 블록 스토리지 볼륨에 대한 정보를 얻으려는 경우**

  * `ibmcloud is volumes`
  
* **블록 스토리지 볼륨에 대한 세부사항을 보려는 경우**

  * `ibmcloud is volume [volume_id]`
