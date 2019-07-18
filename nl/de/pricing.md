---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-29"

keywords: pricing, billing, data, instance, VSI, block, storage, paygo, transfer, floating, server, VPC, allowance, gateway, egress, minimal charges, ARP, traffic

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}


# Preisstruktur für VPC
{: #pricing-for-vpc}

In der Tabelle sind die Preise für die Übertragung von Internetdaten mit {{site.data.keyword.cloud}} Virtual Private Cloud zusammengefasst. Beachten Sie, dass für den Datenverkehr zwischen VPC und anderen klassischen IBM Cloud-Services keine Gebühren erhoben werden. Auch für PayGo-Services sind die Serviceebenen an Ihr Konto und nicht an eine bestimmte VPC-Instanz gebunden. Die Beträge werden in US-Dollar angezeigt.

Für [virtuelle Serverinstanzen](/docs/vpc-on-classic?topic=vpc-on-classic-pricing-for-virtual-servers-for-vpc) und den in IBM Cloud VPC verwendeten [Blockspeicher](/docs/vpc-on-classic?topic=vpc-on-classic-block-storage-pricing) gelten getrennte Preisstrukturen.

## Freibeträge für die Datenübertragung über das Internet
{: #free-allowances-for-internet-data-transfer}

| Datenübertragung |  Kosten für alle IBM Cloud-VPC-Kunden |
|---------------|------------------|
| Innerhalb der Zone | Kostenlos |
| Zwischen Zonen in derselben Region | Kostenlos |
| Verwendung des öffentlichen Gateways | Kostenlos (Gebühren nur für die vom PWG verwendete variable IP-Adresse) |

## Preisstruktur für variable IP-Adressen
{: #floating-ip-pricing}

Eine variable IP-Adresse wird mit einem Gebührensatz von 1 USD pro Monat ab dem Zeitpunkt der Reservierung berechnet. Die Gebühr wird auch dann erhoben, wenn die variable IP-Adresse nicht einer VSI zugeordnet ist oder nicht verwendet wird. Die monatliche Gebühr von 1 USD wird auch dann berechnet, wenn die variable IP-Adresse für nur ein paar Tage reserviert ist.

## Grundlegende Preisstruktur für PayGo für die Datenübertragung über das Internet
{: #basic-paygo-pricing-for-internet-transfer}

| Datenübertragung | Datenvolumen | PayGo-Preisstruktur |
|-----------|-----------|------------------|
| Ausgehend zum Internet |  0 bis 5 GB | Kostenlos |
|  | 6 bis 10.000 GB | 0,087 USD pro GB |
|  | 10.001 bis 50.000 GB | 0,083 USD pro GB |
|  | 50.001 bis 150.000 GB | 0,07 USD pro GB |
|  | 150.001 GB und mehr | 0,05 USD pro GB |


Wenn Sie eine neue VPC erstellen, kann es bis zu einer Stunde dauern, bis die Gebühren für die Anfangsabrechnung in der Konsolenbenutzerschnittstelle oder in der API angezeigt werden.
{: tip}

Wenn Sie ein öffentliches Gateway oder eine variable IP-Adresse verwenden, können einige geringe Gebühren für den abgehenden Datenverkehr ins Internet angezeigt werden, auch wenn Sie keinen abgehenden Datenverkehr zu diesem Zeitpunkt gesendet haben. Diese Gebühren fallen für den ARP-Datenverkehr an, der für das Betreiben Ihres Kontos notwendig ist.
{: important}

## Preisstruktur für LBaaS für VPC
{: #lb-for-vpc-pricing}

Die Preisstruktur für Lastausgleichsfunktionen für VPC basiert auf den folgenden Metriken, die monatlich berechnet werden:
* Stunden der Servicenutzung
* Verarbeitete Daten


| Region | Nutzung des Service für Lastausgleich (pro Stunde) | Pro Gigabyte verarbeitete Daten der Lastausgleichsfunktion (GB) |
|------------|--------------------------|-------------------------|
| Dallas | 0,025 USD | 0,008 USD |
| Frankfurt | 0,027 USD | 0,009 USD |
| Tokio | 0,028 USD | 0,009 USD |

Die Preise variieren in Regionen mit mehreren Zonen.
{: note}

Im folgenden Diagramm wird ein Beispiel für einen Kunden aufgeführt, der pro Monat 4500 GB für öffentlichen Lastausgleich verwendet; der Preis wird in USD angegeben und der Standort ist die Region Dallas. 

| Artikel | Nutzung | Satz | Kosten |
|---------|--------|---------|---------|          
| Nutzung Service für Lastausgleich | 720 Stunden | USD 0,025/Stunde | USD 18,0 pro Monat |
| Verarbeitete Daten | 4.500 GB | USD 0,008/GB | USD 36,0 pro Monat |

**Tabelle 1: Monatliches Kostenbeispiel. Die Gesamtgebühr für dieses Szenario beträgt USD 54 pro Monat.**


## Preisstruktur für VPN für VPC
{: #vpn-for-vpc-pricing}

| Region | Verbindung (Peer) pro Stunde | Instanz (Gateway) pro Stunde |
|------------|--------------------------|-------------------------|
| Dallas | 0,04 USD | 0,045 USD |
| Frankfurt | 0,044 USD | 0,0495 USD |
| Tokio | 0,0452 USD | 0,0585 USD |

Die Datenübertragung ins Internet als Ergebnis einer VPNaaS-Nutzung wird als reguläre Datenübertragung (abgehend ins Internet) abgerechnet; die Abrechnung wird auf VPC-Ebene durchgeführt.
{: note}
