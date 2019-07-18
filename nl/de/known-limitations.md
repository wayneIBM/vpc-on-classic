---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-12"

keywords: limitations, bugs, known, known issues, Beta, services, capabilities, use cases

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

# Bekannte Einschränkungen
{: #known-limitations}

Dieses Dokument enthält eine Zusammenfassung der Features und APIs, die nicht unterstützt werden, Features und Anwendungsfälle, die noch nicht unterstützt werden sowie der bekannten Probleme im aktuellen Release und eine Liste der Funktionen, die nur als Betaservices angeboten werden. Bekannte Einschränkungen können sich ändern, wenn Funktionen zu {{site.data.keyword.vpc_full}} (VPC) hinzugefügt werden; es empfiehlt sich daher, dieses Dokument von Zeit zu Zeit zu überprüfen.

## Zusammenfassung der nicht unterstützten Funktionen
{: #summary-of-features-not-supported}

* Direct Link-Zugriff auf VPC wird ausschließlich über den [**klassischen Zugriff**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc) unterstützt.
* Für VPC steht zwar eine Untergruppe der privaten Serviceendpunkte zur Verfügung, es sind jedoch nicht alle Endpunkte verfügbar. Informationen zu den verfügbaren Services finden Sie in [Für IBM Cloud VPC verfügbare Serviceendpunkte](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc).
* Das Speichern und Wiederherstellen von Images wird nicht unterstützt.
* Da {{site.data.keyword.cloud_notm}} VPC-Instanzen regionsbezogen sind, kann von einer VPC-Instanz aus einer Region nur eine Verbindung zu einer VPC-Instanz in einer anderen Region hergestellt werden, wenn der "klassische Zugriff" aktiviert ist oder eine VPN-Verbindung verwendet wird. Informationen hierzu finden Sie unter [**Klassischer Zugriff**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

## Noch nicht unterstützte Funktionen und Anwendungsfälle
{: #features-and-use-cases-not-yet-supported}

Im folgenden Abschnitt werden weitere Details zu nicht unterstützten Features und Anwendungsfällen aufgelistet, die nach ihrem {{site.data.keyword.cloud_notm}}-Service kategorisiert sind.

### Virtual Private Cloud
{: vpc-unsupported-features-and-use-cases}

* Von {{site.data.keyword.vpc_short}} werden keine Multicast- oder Broadcast-Domänen unterstützt.
* {{site.data.keyword.vpc_short}} bietet keine End-to-End-Verschlüsselung.
* Eine VPC-Instanz kann nicht als Peer mit anderen VPC-Instanzen verwendet werden.
* Von VPC werden keine 'Cloud Service Endpoints' unterstützt. Informationen zu den verfügbaren Services finden Sie in [Für IBM Cloud VPC verfügbare Serviceendpunkte](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc).

### Datenverarbeitung
{: VSI-unsupported-features-and-use-cases}

* Vorhandene {{site.data.keyword.cloud_notm}}-VSIs (VSI - Virtual Server Instance) oder Bare-Metal-Server können nicht zu einer VPC migriert werden.
* Bare-Metal-Server können nicht in einer VPC-Instanz bereitgestellt werden.

### Netz
{: network-unsupported-features-and-use-cases}

* Angepasste Routen können nicht zu einer VPC-Instanz hinzugefügt werden. Alle Teilnetze in einer VPC können standardmäßig miteinander kommunizieren.
* Ein Teilnetz kann sich nicht in mehreren Zonen befinden.
* Ein Teilnetz kann nicht von einer Zone in eine andere verschoben werden.
* Die Größe der Teilnetze kann nach ihrer Erstellung nicht geändert werden.
* Klassische {{site.data.keyword.cloud_notm}}-Ressourcen können sich nicht in demselben Teilnetz in einer VPC befinden. Die Konnektivität zwischen klassischen Ressourcen und einer VPC in Ihrem Konto ist mit dem [**klassischen Zugriff**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc) möglich.
* IPv6 wird nicht unterstützt.
* Sie können Ihre eigene öffentliche IP nicht als variable IP-Adresse verwenden. Der Pool der variablen IP-Adressen wird von IBM bereitgestellt.
* Eine variable IP-Adresse ist an eine Zone gebunden. Beispielsweise kann eine variable IP-Adresse, die aus einem Pool in Zone 1 reserviert ist, nicht einer VSI in Zone 2 in derselben VPC zugeordnet werden.
* Private IP-Adressen können nicht zwischen VSIs verschoben werden.
* Netzschnittstellen können nicht zwischen VSIs verschoben werden.
* Sie können die spezifische IP-Adresse der VSI nicht angeben. Sie können nur den IP-Bereich für das Teilnetz angeben. Sobald Sie eine VSI an ein Teilnetz angeschlossen haben, ordnet das System eine private IP aus diesem Teilnetz zu.
* {{site.data.keyword.vpc_short}} verfügt über eigene 'Network as a Service'-Tools wie LBaaS, ACLs und Sicherheitsgruppen. Sie können keine eigenen Netzgeräte (z. B. ein Vyatta Gateway oder eine andere Lastausgleichsfunktion) verwenden, um alle Ressourcen in der VPC zu steuern. Analog können Sie auch nicht eigene Services für Lastausgleichsfunktionen oder Sicherheitsgruppen in der klassischen {{site.data.keyword.cloud_notm}}-Infrastruktur verwenden, um Ressourcen in der {{site.data.keyword.vpc_short}}-Instanz zu steuern.
* Das öffentliche Gateway lässt nicht zu, dass der Datenverkehr vom Internet zu einer VSI in IBM Cloud VPC initiiert wird. Zu diesem Zweck muss eine variable IP-Adresse verwendet werden.
* ACL ist statusunabhängig, sodass der zurückfließende Datenverkehr explizit in ACL-Regeln zugelassen werden muss.

## Bekannte Probleme in diesem Release
{: #known-issues-in-this-release}

Es kann vorkommen, dass der Datenträgername von {{site.data.keyword.block_storage_is_short}} nicht präzise auf Eindeutigkeit überprüft wird, wenn alle folgenden Bedingungen zutreffen:

* Die Bereitstellungsanforderung läuft gleichzeitig ab
* Der Datenträger weist denselben Namen auf
* Der Datenträger befindet sich in derselben Region

## Verfügbare Betaservices

Das sichere Peering für eine Appliance ist als Betaservice verfügbar. Dokumentation für das Peering mit verschiedenen Appliancetypen ist verfügbar:

* [Ferner Vyatta-Peer](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-vyatta-peer)
* [Ferner StrongSwan-Peer](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-strongswan-peer)
* [Ferner FortiGate-Peer](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-fortigate-peer)
* [Ferner Juniper vSRX-Peer](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-juniper-vsrx-peer)
* [Ferner Cisco ASAv-Peer](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-cisco-asav-peer)
