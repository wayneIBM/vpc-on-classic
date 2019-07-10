---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-07"
keywords: quota, resource, classic, access, gateway, address, prefix, VSI, vNIC, floating, SSH, key, security, group, rule, remote, peer, ACL, region, ingress, egress, VPN, policies, load balancer, listener, pool, per

subcollection: vpc-on-classic

---
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Quotas
{: #quotas}

Ce document présente les quotas et les limites qui s'appliquent dans votre cloud privé virtuel {{site.data.keyword.cloud}} ainsi que les ressources disponibles.

## Quotas pour les clouds privés virtuels
{: #vpc-quotas}

Les comptes doivent respecter les quotas suivants :

|   Ressource     | Nombre maximal |
| ------- | :------: |
| Clouds privés virtuels | 5 par région|
| Clouds privés virtuels avec accès classique | 1 par région |
| Passerelles publiques | 1 par cloud privé virtuel et par zone |
| Préfixes d'adresse | 5 par cloud privé virtuel et par zone |

## Quotas pour les sous-réseaux
{: #subnet-quotas}

|   Ressource     | Nombre maximal |
| ------- | :------: |
| Sous-réseaux | 15 par cloud privé virtuel |


## Quotas pour les instances de serveur virtuel
{: #vsi-quotas}

|   Ressource     | Nombre maximal |
| ------- | :------: |
| Instances de serveur virtuel | 100 par compte par défaut |
| Cartes d'interface réseau virtuel par instance de serveur virtuel | 5 par instance de serveur virtuel |
| Adresses IP flottantes | 1 par instance de serveur virtuel |
| Clés SSH | 100 par compte |


## Quotas pour les groupes de sécurité
{: #security-groups-quotas}

Il existe des quotas basés sur les comptes (limites) pour les groupes de sécurité et leurs règles associées.

|Ressource|Quota|
|--------|-----|
|Groupes de sécurité|5 par interface réseau|
|Groupes de sécurité|500 par compte|
|Règles|50 par groupe de sécurité|
|Règles distantes |5 par groupe de sécurité|
|Interfaces réseau|100 par groupe de sécurité|

## Quotas pour les listes de contrôle d'accès
{: #acl-quotas}

|Ressource|Quota|
|--------|-----|
|Listes de contrôle d'accès| 30 par région |
|Règles d'entrée|20 par liste de contrôle d'accès |
|Règles de sortie |20 par liste de contrôle d'accès |

## Quotas pour les réseaux privés virtuels
{: #vpn-quotas}

Limitations relatives aux ressources de réseau privé virtuel en cours par compte :

|Ressource|Quota|
|--------|-----|
| Passerelles VPN| 20 par compte |
| Passerelles VPN | 3 par zone |
| Connexions VPN | 10 par passerelle VPN |
| Stratégies IKE | 20 par compte |
| Stratégies IPSec | 20 par compte |
| Sous-réseaux homologues sur une passerelle VPN unique | 50 sur toutes les connexions VPN|
| Sous-réseaux homologues  | 15 sur une connexion VPN unique|
| Sous-réseaux locaux sur une passerelle VPN unique | 50 sur toutes les connexions VPN|
| Sous-réseaux locaux |  15 sur une connexion VPN unique |

## Quotas pour les équilibreurs de charge
{: #load-balancer-quotas}

Quotas pour les ressources d'équilibreur de charge en cours :

|Ressource|Quota|
|--------|-----|
| Equilibreurs de charge | 20 par compte |
| Ecouteurs | 10 par équilibreur de charge |
| Pools | 10 par équilibreur de charge |
| Membres | 50 par pool |

## Quotas pour les volumes secondaires
{: #secondary-volume-quotas}

| Ressource | Quota |
|--------|----- |
| Volumes secondaires par instance, lors de la création d'une instance |  4 volumes secondaires peuvent être demandés |
| Volumes secondaires par instance, pour les instances existantes avec moins de 4 coeurs | 4 volumes secondaires |
| Volumes secondaires par instance, pour les instances existantes avec 4 coeurs ou plus | Jusqu'à 12 volumes secondaires |


