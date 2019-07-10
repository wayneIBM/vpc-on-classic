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

# Guida di riferimento rapido ai comandi della CLI per le risorse
{: #quick-reference-to-cli-commands-for-resources}

Questa guida rapida ai comandi della CLI per le risorse è utile per andare avanti con la CLI {{site.data.keyword.cloud}} per Virtual Private Cloud.

* **Per accedere**

  * `ibmcloud login [-sso]`

* **Per ottenere informazioni sui VPC**

  * `ibmcloud is vpcs`
  
Utilizza l'ID risorsa per visualizzare dettagli specifici sulla risorsa.
{: tip}

* **Per visualizzare i dettagli di VPC** 

  * `ibmcloud is vpc [vpc_id]` 

* **Per ottenere informazioni sulle sottoreti** 

  * `ibmcloud is subnets`

* **Per ottenere i dettagli della sottorete**

  * `ibmcloud is subnet [subnet_id]`

* **Per ottenere informazioni sulle istanze**

  * `ibmcloud is instances` 

* **Per visualizzare i dettagli delle istanze** 

  * `ibmcloud is instance [instance_id]`

* **Per ottenere informazioni sugli IP mobili** 

  * `ibmcloud is floating-ips`  

* **Per visualizzare i dettagli dell'IP mobile**

  * `ibmcloud is floating-ip [floating-ips_id]`

* **Per ottenere informazioni sulle regioni e sugli endpoint**

  * `ibmcloud is regions`

* **Per ottenere informazioni sulle zone** 

  * `ibmcloud is zones [region_name]`
  
* **Per ottenere informazioni su tutti i volumi di archiviazione blocchi**

  * `ibmcloud is volumes`
  
* **Per visualizzare i dettagli su un volume di archiviazione blocchi**

  * `ibmcloud is volume [volume_id]`
