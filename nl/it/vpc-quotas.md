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

# Quote
{: #quotas}

Questo documento analizza le quote e i limiti per il tuo {{site.data.keyword.cloud}} Virtual Private Cloud e le risorse disponibili al suo interno.

## Quote di VPC
{: #vpc-quotas}

Gli account hanno le seguenti quote:

|   Risorsa     | Numero massimo |
| ------- | :------: |
| VPC (Virtual Private Cloud) | 5 per regione|
| VPC con Accesso classico | 1 per regione |
| Gateway pubblici (PGW) | 1 per VPC per zona |
| Prefissi indirizzo | 5 per VPC per zona |

## Quote di sottorete
{: #subnet-quotas}

|   Risorsa     | Numero massimo |
| ------- | :------: |
| Sottoreti | 15 per VPC (Virtual Private Cloud) |


## Quote di VSI
{: #vsi-quotas}

|   Risorsa     | Numero massimo |
| ------- | :------: |
| Istanze del server virtuale (VSI) | 100 per account per impostazione predefinita |
| vNIC per VSI | 5 per VSI |
| Indirizzi IP mobili | 1 per VSI |
| Chiavi SSH | 100 per account |


## Quote dei gruppi di sicurezza
{: #security-groups-quotas}

Esistono alcune quote (limiti) basate sull'account per i gruppi di sicurezza e le loro regole associate.

|Risorsa|Quota|
|--------|-----|
|Gruppi di sicurezza|5 per interfaccia di rete|
|Gruppi di sicurezza|500 per account|
|Regole|50 per gruppo di sicurezza|
|Regole remote |5 per gruppo di sicurezza|
|Interfacce di rete|100 per gruppo di sicurezza|

## Quote di ACL
{: #acl-quotas}

|Risorsa|Quota|
|--------|-----|
|ACL| 30 per regione |
|Regole di ingresso|20 per ACL |
|Regole di uscita |20 per ACL |

## Quote di VPN
{: #vpn-quotas}

Di seguito sono riportate le attuali limitazioni delle risorse VPN per account:

|Risorsa|Quota|
|--------|-----|
| Gateway VPN| 20 per account |
| Gateway VPN | 3 per zona |
| Connessioni VPN | 10 per gateway VPN |
| Politiche IKE | 20 per account |
| Politiche IPSec | 20 per account |
| Sottoreti peer su qualsiasi singolo gateway VPN | 50 su tutte le connessioni VPN|
| Sottoreti peer  | 15 su qualsiasi singola connessione VPN|
| Sottoreti locali su qualsiasi singolo gateway VPN | 50 su tutte le connessioni VPN|
| Sottoreti locali |  15 su qualsiasi singola connessione VPN |

## Quote del programma di bilanciamento del carico
{: #load-balancer-quotas}

Di seguito sono riportate le quote attuali delle risorse del programma di bilanciamento del carico:

|Risorsa|Quota|
|--------|-----|
| Programmi di bilanciamento del carico | 20 per account |
| Listener | 10 per programma di bilanciamento del carico |
| Pool | 10 per programma di bilanciamento del carico |
| Membri | 50 per pool |

## Quote del volume secondario
{: #secondary-volume-quotas}

| Risorsa | Quota |
|--------|----- |
| Volumi secondari per istanza, quando si crea un'istanza | Possono essere richiesti 4 volumi secondari |
| Volumi secondari per istanza, per istanze esistenti con meno di 4 core | 4 volumi secondari |
| Volumi secondari per istanza, per istanze esistenti con 4 o più core | Fino a 12 volumi secondari |


