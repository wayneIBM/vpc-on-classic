---

copyright:
  years: 2019
lastupdated: "2019-05-21"

keywords: storage, capacity, billing, volume

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:new_window: target="_blank"}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Preisstruktur für Block Storage for VPC
{: #block-storage-pricing}

Die Preisstruktur für {{site.data.keyword.block_storage_is_short}} basiert auf der Kapazität oder den E/A-Operationen pro Sekunde, die bereitgestellt wurden, und hängt vom jeweils gewählten Speicherprofil ab. 

| IOPS-Tier  | Veröffentlichter monatlicher Satz^1 |
|------------|--------------|
|  3 IOPS/GB |  0,13 USD/GB |
|  5 IOPS/GB |  0,25 USD/GB |
| 10 IOPS/GB |  0,58 USD/GB |
| Angepasste IOPS| 0,10 USD/GB + 0,07 USD/IOPS |

^1 Veröffentlichter Preis pro Monat, [stündlich berechnet](#how-are-charges-calculated).

## Wie werden die Gebühren berechnet?
{: #how-are-charges-calculated}

Die Gebühren für {{site.data.keyword.block_storage_is_short}} werden stündlich auf der Basis des Gesamtzeitraums der Stunden berechnet, die der Blockspeicherdatenträger für das Konto vorhanden ist, bis der Datenträger gelöscht wird oder das Ende des Rechnungsstellungszyklus erreicht wird (das frühere der beiden Ereignisse beendet den Zyklus). Die stündliche Abrechnung angepasster E/A-Operationen pro Sekunde (IOPS) weicht von der stündlichen Abrechnung für ein vordefiniertes IOPS-Tier ab. Weitere Informationen finden Sie in den folgenden Beispielen.

### Berechnung einer Rechnungsstellung für einen Datenträger mit Universal-IOPS-Tier
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

Ein Datenträger mit 1000 GB wird mit Universal-Tier und 3 IOPS/GB bereitgestellt, der Datenträger wird 72 Stunden verwendet und anschließend gelöscht. Der Gesamtpreis für den Datenträger wird nach Stunden wie folgt berechnet:

((1000 GB x 0,13 USD/GB)/730 Std.) x 72 Std. = 12,82 USD

### Berechnung einer Rechnungsstellung für einen Datenträger mit angepassten IOPS
{: #billing-calculation-for-a-volume-with-custom-IOPS}

Ein Datenträger mit 1000 GB wird mit 2500 IOPS/GB bereitgestellt, der Datenträger wird 72 Stunden verwendet und anschließend gelöscht. Der Gesamtpreis für den Datenträger wird nach Stunden wie folgt berechnet:

(((1000 x 0,10 USD/GB) + (2500 x 0,07 USD)) / 730 Std.) x 72 Std. = 27,12 USD
