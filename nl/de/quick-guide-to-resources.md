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

# Kurzübersicht zu CLI-Befehlen für Ressourcen
{: #quick-reference-to-cli-commands-for-resources}

Diese Kurzanleitung zu CLI-Befehlen für Ressourcen dient als hilfreiche Einführung in die {{site.data.keyword.cloud}}-CLI für Virtual Private Cloud (VPC).

* **Anmeldung**

  * `ibmcloud login [-sso]`

* **Informationen zu VPCs abrufen**

  * `ibmcloud is vpcs`
  
Verwenden Sie die Ressourcen-ID, um bestimmte Details zu der Ressource anzuzeigen.
{: tip}

* **VPC-Details anzeigen** 

  * `ibmcloud is vpc [vpc-id]` 

* **Informationen zu Teilnetzen abrufen** 

  * `ibmcloud is subnets`

* **Details zu Teilnetzen abrufen**

  * `ibmcloud is subnet [teilnetz-id]`

* **Informationen zu Instanzen abrufen**

  * `ibmcloud is instances` 

* **Details zu Instanzen anzeigen** 

  * `ibmcloud is instance [instanz-id]`

* **Informationen zu variablen IP-Adressen abrufen** 

  * `ibmcloud is floating-ips`  

* **Details zu variablen IP-Adressen abrufen**

  * `ibmcloud is floating-ip [id_der_variablen_ip-adressen]`

* **Informationen zu Ihren Regionen und Endpunkten abrufen**

  * `ibmcloud is regions`

* **Informationen zu Zonen abrufen** 

  * `ibmcloud is zones [regionsname]`
  
* **Informationen zu allen Blockspeicherdatenträgern abrufen**

  * `ibmcloud is volumes`
  
* **Details zu einem Blockspeicherdatenträger anzeigen**

  * `ibmcloud is volume [volume_id]`
