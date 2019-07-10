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

# Noeuds finaux de service disponibles pour IBM Cloud VPC
{: #service-endpoints-available-for-ibm-cloud-vpc}

Dès que vous êtes prêt à exécuter des charges de travail, vous pouvez accéder à deux types de noeuds finaux : des noeuds finaux IaaS (Infrastructure as a Service) et CSE (Cloud Service Endpoints). Les noeuds finaux IaaS sont déjà fournis et prêts à l'emploi, contrairement aux noeuds finaux CSE qui doivent être mis à disposition séparément pour pouvoir être utilisés.

Les noeuds finaux correspondant à ces services utilisent des _adresses routables_, c'est-à-dire des adresses en dehors de celles indiquées dans le document RFC 1918, et même si ces adresses semblent communiquer via l'Internet public, le trafic en provenance et à destination de ces noeuds finaux ne sort pas du cloud. Par conséquent, ce trafic évite les frais de bande passante associés au trafic qui quitte le cloud et passe par l'Internet public.

## Noeuds finaux IaaS (Infrastructure as a Service)
{: #infrastructure-as-a-service-iaas-endpoints}

Les services d'infrastructure sont disponibles en utilisant des noms DNS particuliers issus du domaine `adn.networklayer.com` et résolus en adresses `161.26.0.0/16`.

Vous pouvez accéder aux services suivants :

* Programmes de résolution DNS
* Miroirs APT (Advanced Packaging Tool) d'Ubuntu
* Cloud Object Storage
* NTP (Network Time Protocol)

## Noeuds finaux des programmes de résolution DNS (Domain Name System)
{: #dns-domain-name-system-resolver-endpoints}

Les programmes de résolution DNS utilisent des adresses IP plutôt que des noms. Pour les noeuds finaux de service cloud (CSE) partagés, les adresses de serveur DNS `161.26.0.10` et `161.26.0.11` doivent être utilisées.

## Miroirs APT d'Ubuntu
{: #ubuntu-apt-mirrors}

Les miroirs APT utilisés pour mettre à jour des images Ubuntu sont disponibles dans le domaine `mirrors.adn.networklayer.com` et résolus en adresses `161.26.0.6`. 

## IBM Cloud Object Storage
{: #ibm-cloud-object-storage}

Par exemple, pour atteindre un noeud final privé d'un service de stockage d'objets sur le réseau privé d'IBM Cloud, remplacez la partie du "domaine" de l'URL par `cloud-object-storage.appdomain.cloud`. Pour atteindre un compartiment COS créé dans une autre région,
utilisez le noeud final du compartiment et non pas celui de la région dans laquelle a été créée l'instance de serveur virtuel.

## Serveurs NTP (Network Time Protocol)
{: #network-time-protocol-ntp-servers}

Un serveur NTP est disponible dans le domaine `time.adn.networklayer.com`, qui doit être résolu en adresse `161.26.0.6`.

## Noeuds finaux CSE (Cloud Service Endpoints)
{: #cloud-service-endpoints}

Les noeuds finaux de service cloud (noeuds CSE) sont des services fournis par d'autres utilisateurs du cloud. Ils seront bientôt disponibles avec des noms DNS dans le domaine `cloud.ibm.com`. Ils sont résolus en adresses `166.8.0.0/14`.
