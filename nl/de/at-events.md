---

copyright:
  years: 2019

lastupdated: "2019-06-13"

keywords: activity tracker, vpc, events, logdna 

subcollection: vpc-on-classic

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:download: .download}


# Activity Tracker mit LogDNA-Ereignissen
{: #at-events}

Verwenden Sie den {{site.data.keyword.at_full}}-Service, um zu verfolgen, wie Benutzer und Anwendungen mit {{site.data.keyword.cloud}} Virtual Private Cloud (VPC) in {{site.data.keyword.cloud}} interagieren.
{:shortdesc}

Vom {{site.data.keyword.at_full}}-Service werden vom Benutzer eingeleitete Aktivitäten aufgezeichnet, durch die der Status eines Service in {{site.data.keyword.cloud}} geändert wird. Weitere Informationen finden Sie unter [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).

## Liste der Ereignisse: Netzressourcen
{: #events-volumes}

| Ressource  | Aktion  | Beschreibung  |
|:----------------|:-----------------------|:-----------------------|
| vpc  | is.vpc.vpc.create   | VPC wurde erstellt.  |
| vpc  | is.vpc.vpc.update   | VPC wurde aktualisiert.  |
| vpc  | is.vpc.vpc.delete   | VPC wurde gelöscht.  |
| vpc  | is.vpc.address-prefix.create  | Adresspräfix wurde zu VPC hinzugefügt.  |
| vpc  | is.vpc.address-prefix.update  | VPC-Adressenpräfix wurde aktualisiert.  |
| vpc  | is.vpc.address-prefix.delete  | Adressenpräfix wurde aus VPC entfernt.  |
| vpc  | is.vpc.vpc-route.create   | Weiterleitung wurde zu VPC hinzugefügt.  |
| vpc  | is.vpc.vpc-route.update   | VPC-Weiterleitung wurde aktualisiert.  |
| vpc  | is.vpc.vpc-route.delete   | Weiterleitung wurde aus VPC entfernt.  |
| floating-ip  | is.floating-ip.floating-ip.delete   | Variable IP-Adresse wurde erstellt.  |
| floating-ip  | is.floating-ip.floating-ip.delete   | Variable IP-Adresse wurde aktualisiert.  |
| floating-ip  | is.floating-ip.floating-ip.delete   | Variable IP-Adresse wurde gelöscht.  |
| network-acl  | is.network-acl.network-acl.create   | Netz-ACL wurde erstellt.  |
| network-acl  | is.network-acl.network-acl.update   | Netz-ACL wurde aktualisiert.  |
| network-acl  | is.network-acl.network-acl.delete   | Netz-ACL wurde gelöscht.  |
| network-acl  | is.network-acl.rule.create  | Regel wurde zu Netz-ACL hinzugefügt.  |
| network-acl  | is.network-acl.rule.update  | Netz-ACL-Regel wurde aktualisiert.  |
| network-acl  | is.network-acl.rule.delete  | Regel wurde aus Netz-ACL entfernt.  |
| public-gateway | is.public-gateway.public-gateway.create   | Öffentliches Gateway wurde erstellt.  |
| public-gateway | is.public-gateway.public-gateway.update   | Öffentliches Gateway wurde aktualisiert.  |
| public-gateway | is.public-gateway.public-gateway.delete   | Öffentliches Gateway wurde gelöscht.  |
| security-group | is.security-group.security-group.create   | Sicherheitsgruppe erstellt |
| security-group | is.security-group.security-group.delete   | Sicherheitsgruppe gelöscht |
| security-group | is.security-group.security-group.update   | Sicherheitsgruppe aktualisiert  |
| security-group | is.security-group.security-group.rule-create  | Sicherheitsgruppenregel erstellt  |
| security-group | is.security-group.security-group.rule-delete  | Sicherheitsgruppenregel gelöscht  |
| security-group | is.security-group.security-group.rule-update  | Sicherheitsgruppenregel aktualisiert  |
| security-group | is.security-group.security-group.interface-attach | Sicherheitsgruppenschnittstelle zugeordnet  |
| security-group | is.security-group.security-group.interface-detach | Zuordnung der Sicherheitsgruppenschnittstelle aufgehoben  |
| subnet   | is.subnet.subnet.create   | Teilnetz wurde erstellt.  |
| subnet   | is.subnet.subnet.update   | Teilnetz wurde aktualisiert.  |
| subnet   | is.subnet.subnet.delete   | Teilnetz wurde gelöscht.  |
| subnet   | is.subnet.network-acl.update  | Netz-ACL des Teilnetzes wurde ersetzt.  |
| subnet   | is.subnet.public-gateway.operate  | Öffentliches Gateway wurde Teilnetz zugeordnet.  |
| subnet   | is.subnet.public-gateway.operate  | Zuordnung des öffentlichen Gateways zum Teilnetz wurde aufgehoben.  |

## Liste der Ereignisse: Rechenressourcen
{: #events-computes}

In der folgenden Tabelle werden die Aktionen aufgelistet, die Rechenressourcen und die Generierung von Ereignissen betreffen.

| Ressource  | Aktion  | Beschreibung  |
|:----------------|:-----------------------|:-----------------------|
| instance   | is.instance.instance.create   | Instanz wurde erstellt.  |
| instance   | is.instance.instance.delete   | Instanz wurde gelöscht.  |
| instance   | is.instance.instance.update   | Instanz wurde aktualisiert.  |
| instance   | is.instance.action.create   | Instanzaktion wurde erstellt.  |
| instance   | is.instance.action.delete   | Anstehende Instanzaktion wurde gelöscht.  |
| instance   | is.instance.network_interface.floating_ip.create  | Variable IP-Adresse wurde der Netzschnittstelle der Instanz zugeordnet.  |
| instance   | is.instance.network_interface.floating_ip.delete  | Zuordnung der variablen IP-Adresse zur Netzschnittstelle der Instanz wurde aufgehoben.  |
| instance   | is.instance.volume_attachement.create   | Datenträgerzuordnung der Instanz wurde erstellt.  |
| instance   | is.instance.volume_attachement.delete   | Datenträgerzuordnung der Instanz wurde gelöscht.  |
| instance   | is.instance.volume_attachement.update   | Datenträgerzuordnung der Instanz wurde aktualisiert.  |
| key  | is.key.key.create   | Schlüssel wurde erstellt.  |
| key  | is.key.key.delete   | Schlüssel wurde gelöscht.  |
| key  | is.key.key.update   | Schlüssel wurde aktualisiert.  |

## Liste der Ereignisse: Speicherressourcen
{: #events-storage}

In der folgenden Tabelle werden die Aktionen aufgelistet, die Speicherressourcen und die Generierung von Ereignissen betreffen.

| Ressource  | Aktion  | Beschreibung  |
|:----------------|:-----------------------|:-----------------------|
| volume  | is.volume.volume.create  | Datenträger wurde erstellt.  |
| volume  | is.volume.volume.update  | Datenträger wurde aktualisiert.  |
| volume  | is.volume.volume.delete  | Datenträger wurde gelöscht.  |


## Liste der Ereignisse: Lastausgleichsfunktionen
{: #events-load-balancers}

In der folgenden Tabelle werden die Aktionen aufgelistet, die Lastausgleichsfunktionen und die Generierung von Ereignissen betreffen.

| Ressource  | Aktion  | Beschreibung  |
|:----------------|:-----------------------|:-----------------------|
| Lastausgleichsfunktion |  is.load-balancer.load-balancer.create | Lastausgleichsfunktion wurde erstellt. |
| Lastausgleichsfunktion |  is.load-balancer.load-balancer.update | Lastausgleichsfunktion wurde aktualisiert. |
| Lastausgleichsfunktion |  is.load-balancer.load-balancer.delete | Lastausgleichsfunktion wurde gelöscht. |
| Listener |  is.load-balancer.load-balancer.listener.create | Listener wurde erstellt. |
| Listener |  is.load-balancer.load-balancer.listener.update | Listener wurde aktualisiert. |
| Listener |  is.load-balancer.load-balancer.listener.delete | Listener wurde gelöscht. |
| Pool |  is.load-balancer.load-balancer.pool.create | Pool wurde erstellt. |
| Pool |  is.load-balancer.load-balancer.pool.update | Pool wurde aktualisiert. |
| Pool |  is.load-balancer.load-balancer.pool.delete | Pool wurde gelöscht. |
| Mitglied |  is.load-balancer.load-balancer.pool.member.create | Mitglied wurde erstellt. |
| Mitglied |  is.load-balancer.load-balancer.pool.member.update | Mitglied wurde aktualisiert. |
| Mitglied |  is.load-balancer.load-balancer.pool.member.delete | Mitglied wurde gelöscht. |
| Richtlinie |  is.load-balancer.load-balancer.listener.policy.create | Richtlinie wurde erstellt. |
| Richtlinie |  is.load-balancer.load-balancer.listener.policy.update | Richtlinie wurde aktualisiert. |
| Richtlinie |  is.load-balancer.load-balancer.listener.policy.delete | Richtlinie wurde gelöscht. |
| Regel |  is.load-balancer.load-balancer.listener.policy.rule.create | Regel wurde erstellt. |
| Regel |  is.load-balancer.load-balancer.listener.policy.rule.update | Regel wurde aktualisiert. |
| Regel |  is.load-balancer.load-balancer.listener.policy.rule.delete | Regel wurde gelöscht. |

Die Prüfereignisse der Lastausgleichsfunktion werden in {{site.data.keyword.at_full}} in der Region `us-south` aufgezeichnet. In welcher Region der Service für die Lastausgleichsfunktion bereitgestellt wird, ist hiervon unabhängig.
{:note}

## Liste der Ereignisse: VPNs
{: #events-vpn}

In der folgenden Tabelle werden die Aktionen aufgelistet, die VPNs und die Generierung von Ereignissen betreffen. 

| Ressource  | Aktion  | Beschreibung  |
|:----------------|:-----------------------|:-----------------------|
| vpn  | is.vpn.vpn.create   | VPN wurde erstellt.  |
| vpn  | is.vpn.vpn.delete   | VPN wurde gelöscht.  |
| vpn  | is.vpn.vpn.update   | VPN wurde aktualisiert.  |
| vpn  | is.vpn.vpn.update   | VPN-Verbindung wurde erstellt.  |
| vpn  | is.vpn.vpn.update   | VPN-Verbindung wurde gelöscht.  |
| vpn  | is.vpn.vpn.update   | VPN-Verbindung wurde aktualisiert.  |


## Unterstützte Standorte
{: #at-supported-locations}

{{site.data.keyword.at_full}}-Unterstützung ist derzeit für die Standorte Dallas und Frankfurt verfügbar. In der folgenden Tabelle wird Activity Tracker mit LogDNA-Standort für jede VPC-Region aufgelistet.

| VPC-Region | Activity Tracker mit LogDNA-Standort |
|--------------|---------------------------------------|
| us-south   | Dallas  |
| jp-tok   | Tokio  |
| eu-de  | Frankfurt   |

## Mögliche Positionen der Ereignisse
{: #at-events-ui}

Wenn Sie eine {{site.data.keyword.at_full}}-Instanz bereitstellen, werden Ereignisse automatisch an Activity Tracker mit einer LogDNA-Serviceinstanz am [unterstützten Standort](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations`) weitergeleitet. 
