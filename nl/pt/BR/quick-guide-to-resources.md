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

# Referência rápida para comandos da CLI para recursos
{: #quick-reference-to-cli-commands-for-resources}

Este guia rápido para comandos da CLI para recursos é útil para começar a usar a CLI do {{site.data.keyword.cloud}} para o Virtual Private Cloud.

* ** Para efetuar login **

  * ` ibmcloud login [ -sso ] `

* ** Para obter informações sobre VPCs **

  * `ibmcloud is vpcs`
  
Use o ID do recurso para visualizar detalhes específicos sobre o recurso.
{: tip}

* ** Para visualizar detalhes do VPC ** 

  * ` ibmcloud é vpc [ vpc_id ] ` 

* ** Para obter informações sobre sub-redes ** 

  * `ibmcloud is subnets`

* ** Para obter detalhes da sub-rede **

  * ` ibmcloud é sub-rede [ subnet_id ] `

* ** Para obter informações sobre instâncias **

  * `ibmcloud is instances
` 

* ** Para visualizar detalhes sobre instâncias ** 

  * ` ibmcloud é a instância [ instance_id ] `

* ** Para obter informações sobre IPs flutuantes ** 

  * ` ibmcloud é flutuante-ips `  

* **Para visualizar detalhes do IP flutuante**

  * ` ibmcloud está flutuando-ip [ floating-ips_id ] `

* **Para obter informações sobre suas regiões e terminais**

  * ` ibmcloud são regiões `

* ** Para obter informações sobre zonas ** 

  * `ibmcloud is zones [region_name]`
  
* **Para obter informações sobre todos os volumes de armazenamento de bloco**

  * `ibmcloud is volumes`
  
* **Para visualizar detalhes sobre um volume de armazenamento de bloco**

  * `ibmcloud is volume [volume_id]`
