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

# Référence rapide aux commandes de l'interface de ligne de commande pour les ressources
{: #quick-reference-to-cli-commands-for-resources}

Ce guide de référence rapide aux commandes de l'interface de ligne de commande pour les ressources est utile si vous vous servez de l'interface de ligne de commande d'{{site.data.keyword.cloud}} pour Virtual Private Cloud.

* **Pour vous connecter**

  * `ibmcloud login [-sso]`

* **Pour obtenir des informations sur les clouds privés virtuels**

  * `ibmcloud is vpcs`
  
Utilisez l'ID de la ressource pour afficher des détails spécifiques sur la ressource.
{: tip}

* **Pour afficher les détails d'un cloud privé virtuel** 

  * `ibmcloud is vpc [vpc_id]` 

* **Pour obtenir des informations sur les sous-réseaux** 

  * `ibmcloud is subnets`

* **Pour obtenir les détails d'un sous-réseau**

  * `ibmcloud is subnet [subnet_id]`

* **Pour obtenir des informations sur les instances**

  * `ibmcloud is instances
` 

* **Pour afficher les détails d'une ou plusieurs instances** 

  * `ibmcloud is instance [instance_id]`

* **Pour obtenir des informations sur les adresses IP flottantes** 

  * `ibmcloud is floating-ips`  

* **Pour afficher les détails d'une ou plusieurs adresses IP flottantes**

  * `ibmcloud is floating-ip [floating-ips_id]`

* **Pour obtenir des informations sur vos régions et noeuds finaux**

  * `ibmcloud is regions`

* **Pour obtenir des informations sur les zones** 

  * `ibmcloud is zones [region_name]`
  
* **Pour obtenir des informations sur tous les volumes de stockage par blocs**

  * `ibmcloud is volumes`
  
* **Pour afficher les détails sur un volume de stockage par blocs**

  * `ibmcloud is volume [volume_id]`
