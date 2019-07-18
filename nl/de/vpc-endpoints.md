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

# Für IBM Cloud VPC verfügbare Serviceendpunkte
{: #service-endpoints-available-for-ibm-cloud-vpc}

Wenn Sie bereit sind, Workloads auszuführen, können Sie zwei Endpunkttypen erreichen: IaaS-Endpunkte (IaaS - Infrastructure as a Service) und Cloud Service Endpoints (CSEs). Die IaaS-Endpunkte werden vorab bereitgestellt und sind sofort verwendbar; die CSEs müssen dagegen separat bereitgestellt werden, damit sie verwendet werden können.

Endpunkte für diese Services sind _weiterleitbare Adressen_, bei denen es sich um Adressen außerhalb des in RFC 1918 angegebenen Adressraums handelt; diese Adressen wirken vielleicht so, als würden sie über das öffentliche Internet kommunizieren, der Datenverkehr von und zu diesen Endpunkten verläuft jedoch in der Cloud. Somit werden durch diesen Datenverkehr Bandbreitengebühren vermieden, die durch Datenverkehr entstehen würde, der die Cloud verlässt und in das öffentliche Internet fließt.

## IaaS-Endpunkte (IaaS - Infrastructure as a Service)
{: #infrastructure-as-a-service-iaas-endpoints}

Infrastrukturservices werden mithilfe bestimmter DNS-Namen aus der Domäne `adn.networklayer.com` bereitgestellt und werden in `161.26.0.0/16`-Adressen aufgelöst.

Zu diesen erreichbaren Services zählen unter anderem:

* DNS-Resolver
* Ubuntu APT Mirrors (APT - Advanced Packaging Tool)
* Cloud Object Storage (COS)
* NTP

## DNS-Resolver-Endpunkte (DNS - Domain Name System)
{: #dns-domain-name-system-resolver-endpoints}

Von DNS-Resolvern werden IP-Adressen anstatt Namen verwendet. Für gemeinsam genutzte Endpunkte von Cloud-Services müssen die DNS-Serveradressen `161.26.0.10` und `161.26.0.11` verwendet werden.

## Ubuntu APT Mirrors
{: #ubuntu-apt-mirrors}

APT Mirrors steht zum Aktualisieren von Ubuntu-Images auf `mirrors.adn.networklayer.com` zur Verfügung, was in `161.26.0.6` aufgelöst werden muss. 

## IBM Cloud Object Storage
{: #ibm-cloud-object-storage}

Wenn Sie z. B. einen privaten Endpunkt für einen Objektspeicherservice im privaten IBM Cloud-Netz erreichen möchten, ersetzen Sie den Abschnitt "Domäne" der URL durch `cloud-object-storage.appdomain.cloud`. Wenn Sie ein COS-Bucket erreichen möchten, das in einer anderen Region erstellt wurde, verwenden Sie den Endpunkt für das Bucket, nicht den Endpunkt der Region, in der die virtuelle Serverinstanz (VSI) erstellt wurde.

## NTP-Server (NTP - Network Time Protocol)
{: #network-time-protocol-ntp-servers}

Ein NTP-Server steht auf `time.adn.networklayer.com` zur Verfügung, was in `161.26.0.6` aufgelöst werden muss.

## Cloud Service Endpoints (CSEs)
{: #cloud-service-endpoints}

Cloud Service Endpoints sind Services, die von anderen Cloudbenutzern bereitgestellt werden. Sie werden in Kürze über DNS-Namen in der Domäne `cloud.ibm.com` verfügbar sein. Sie werden in `166.8.0.0/14`-Adressen aufgelöst.
