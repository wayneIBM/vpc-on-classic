---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-12"

keywords: endpoint, service, DNS, resolver, mirror, object, storage, bandwidth, charges

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

# Endpoint di servizio disponibili per IBM Cloud VPC
{: #service-endpoints-available-for-ibm-cloud-vpc}

Quando sei pronto a eseguire i carichi di lavoro, puoi raggiungere due tipi di endpoint: gli endpoint IaaS (Infrastructure as a Service) e gli endpoint CSE (Cloud Service Endpoint). Gli endpoint IaaS sono sottoposti a pre-provisioning e pronti per l'uso; tuttavia, il provisioning dei CSE deve essere eseguito separatamente prima di poterli utilizzare.

Gli endpoint per questi servizi utilizzano degli _indirizzi instradabili_, cioè indirizzi esterni a quelli specificati in RFC 1918, e tali indirizzi potrebbero sembrare come se stessero comunicando tramite l'Internet pubblico, ma il traffico da e verso questi endpoint non lascia il cloud. Pertanto, questo traffico evita gli addebiti di larghezza di banda associati al traffico che esce dal cloud e va sull'Internet pubblico.

## Endpoint IaaS (Infrastructure as a Service)
{: #infrastructure-as-a-service-iaas-endpoints}

I servizi dell'infrastruttura sono disponibili utilizzando determinati nomi DNS dal dominio `adn.networklayer.com` e vengono risolti in indirizzi `161.26.0.0/16`.

I servizi che puoi raggiungere includono:

* Resolver DNS
* Mirror APT (Advanced Packaging Tool) Ubuntu
* Cloud Object Storage
* NTP

## Endpoint dei resolver DNS (Domain Name System)
{: #dns-domain-name-system-resolver-endpoints}

I resolver DNS utilizzano l'indirizzo IP, piuttosto che i nomi. Per gli endpoint del servizio cloud condivisi, devono essere utilizzati gli indirizzi del server DNS `161.26.0.10` e `161.26.0.11`.

## Mirror APT Ubuntu
{: #ubuntu-apt-mirrors}

I mirror APT per l'aggiornamento delle immagini Ubuntu sono disponibili da `mirrors.adn.networklayer.com`, che dovrebbe essere risolto in `161.26.0.6`. 

## IBM Cloud Object Storage
{: #ibm-cloud-object-storage}

Ad esempio, per raggiungere un endpoint privato per un servizio di archiviazione oggetti sulla rete privata di IBM Cloud, sostituisci la parte "dominio" dell'URL con `cloud-object-storage.appdomain.cloud`. Per raggiungere un bucket COS creato in un'altra regione,
utilizza l'endpoint per il bucket e non l'endpoint della regione in cui è stata creata la VSI.

## Server NTP (Network Time Protocol)
{: #network-time-protocol-ntp-servers}

Un server NTP è disponibile da `time.adn.networklayer.com`, che dovrebbe essere risolto in `161.26.0.6`.

## Endpoint del servizio cloud (CSE)
{: #cloud-service-endpoints}

Gli endpoint del servizio cloud sono servizi forniti da altri utenti del cloud. Saranno presto disponibili attraverso i nomi DNS nel dominio `cloud.ibm.com`. Vengono risolti in indirizzi `166.8.0.0/14`.
