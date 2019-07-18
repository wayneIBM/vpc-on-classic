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

# Kontingente
{: #quotas}

Dieser Abschnitt enthält Informationen zu Kontingenten und Limits für Ihre {{site.data.keyword.cloud}} Virtual Private Cloud-Instanz und die darin verfügbaren Ressourcen.

## VPC-Kontingente
{: #vpc-quotas}

Konten haben folgende Kontingente:

|   Ressource     | Maximale Anzahl |
| ------- | :------: |
| Virtual Private Clouds | 5 pro Region |
| VPCs mit klassischem Zugriff | 1 pro Region |
| Öffentliche Gateways (PGWs) | 1 pro VPC pro Zone |
| Adresspräfixe | 5 pro VPC pro Zone |

## Teilnetzkontingente
{: #subnet-quotas}

|   Ressource     | Maximale Anzahl |
| ------- | :------: |
| Teilnetze | 15 pro VPC |


## VSI-Kontingente
{: #vsi-quotas}

|   Ressource     | Maximale Anzahl |
| ------- | :------: |
| Virtuelle Serverinstanzen (VSIs) | Standardmäßig 100 pro Konto |
| vNICs pro VSI | 5 pro VSI |
| Variable IP-Adressen | 1 pro VSI |
| SSH-Schlüssel | 100 pro Konto |


## Kontingente für Sicherheitsgruppen
{: #security-groups-quotas}

Für Sicherheitsgruppen und die zugehörigen Regeln sind einige kontobasierte Kontingente (Grenzwerte) vorhanden.

|Ressource|Kontingent|
|--------|-----|
|Sicherheitsgruppen|5 pro Netzschnittstelle|
|Sicherheitsgruppen|500 pro Konto|
|Regeln|50 pro Sicherheitsgruppe|
|Ferne Regeln |5 pro Sicherheitsgruppe|
|Netzschnittstellen|100 pro Sicherheitsgruppe|

## ACL-Kontingente
{: #acl-quotas}

|Ressource|Kontingent|
|--------|-----|
|ACLs| 30 pro Region |
|Eingangsregeln|20 pro ACL |
|Ausgangsregeln |20 pro ACL |

## VPN-Kontingente
{: #vpn-quotas}

Nachfolgend finden Sie die aktuellen Beschränkungen für VPN-Ressourcen pro Konto:

|Ressource|Kontingent|
|--------|-----|
| VPN-Gateways| 20 pro Konto |
| VPN-Gateways | 3 pro Zone |
| VPN-Verbindungen | 10 pro VPN-Gateway |
| IKE-Richtlinien | 20 pro Konto |
| IPSec-Richtlinien | 20 pro Konto |
| Peer-Teilnetze für ein beliebiges VPN-Gateway | 50 für alle VPN-Verbindungen|
| Peer-Teilnetze  | 15 für eine einzelne VPN-Verbindung|
| Lokale Teilnetze für ein einzelnes VPN-Gateway | 50 für alle VPN-Verbindungen|
| Lokale Teilnetze |  15 für eine einzelne VPN-Verbindung |

## Kontingente für Lastausgleichsfunktion
{: #load-balancer-quotas}

Nachfolgend finden Sie die aktuellen Ressourcenkontingente für die Lastausgleichsfunktion:

|Ressource|Kontingent|
|--------|-----|
| Lastausgleichsfunktionen | 20 pro Konto |
| Listener | 10 pro Lastausgleichsfunktion |
| Pools | 10 pro Lastausgleichsfunktion |
| Mitglieder | 50 pro Pool |

## Kontingente für sekundäre Datenträger
{: #secondary-volume-quotas}

| Ressource | Kontingent |
|--------|----- |
| Sekundäre Datenträger pro Instanz beim Erstellen einer Instanz | 4 sekundäre Datenträger können angefordert werden |
| Sekundäre Datenträger pro Instanz, für vorhandene Instanzen mit weniger als 4 Kernen | 4 sekundäre Datenträger |
| Sekundäre Datenträger pro Instanz, für vorhandene Instanzen mit 4 Kernen oder mehr | bis zu 12 sekundäre Datenträger |


