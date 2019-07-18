---



copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: pricing, billing, data, instance, VSI, VPC, vCPU, RAM, workload, discount, usage, minimum, invoice, delay, limitation, operating system, suspend

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Preisstruktur für {{site.data.keyword.vsi_is_short}} 
{: #pricing-for-virtual-servers-for-vpc}
[comment]: # (Verknüpfter Hilfeabschnitt)


{{site.data.keyword.vsi_is_full}} wird in ausgewählten Regionen mit bis zu 62 vCPU und 248 GB Arbeitsspeicher angeboten, um allen Workloadanforderungen zu entsprechen. Ihnen wird nur ein Stundensatz in Rechnung gestellt, wobei Sie Rabatte erhalten, je länger Sie Ihre Instanz ausführen. Die Verwendungsdauer virtueller Server wird nach Sekunden berechnet, aufgegliedert nach Zeiten, in denen die Instanzen in Gebrauch sind, und nach Aussetzzeiten der Instanzen. Wenn Ihre Instanz beispielsweise für 45 Minuten und 32 Sekunden aktiv ist, werden Ihnen entsprechend 45 Minuten und 32 Sekunden in Rechnung gestellt.
{:shortdesc}

## Dauernutzung
{: #sustained-usage}

Für Ihre Instanzen wird ein Stundensatz in Rechnung gestellt. Je länger eine Instanz jedoch aktiv ist, desto niedriger ist der Satz. Bei fortschreitendem Rechnungsmonat erhalten Sie die folgenden Stundenrabatte.

| Abgelaufene Zeit im Monat       | Abrechnungsrabatt  | 
| ----------------------------- | ----------------- | 
| 0-20%                         | regulärer Einzelhandelssatz |                 
| 21-40%                        | 5%        |                  
| 41-60%                        | 10%       |                  
| 61-80%                        | 15%        |                  
| 81-100%                       | 20% |
{: caption="Tabelle 1. Gestaffelte Rabatte" caption-side="top"}  

Diese gestaffelte Preisstruktur bietet Ihnen Einsparungen von 10%, wenn Sie Instanzen anstelle von stündlich monatlich aktiv belassen. Dieser Rabatt gilt nur für Basisstundensätze; er gilt nicht für Software-, Speicher-, Netz- oder andere Gebühren.

<!-- As your workload demands change, you can always increase or decrease the size of your instance. If you resize to a larger instance size, the discounts reset and you pay the regular rate again. If you resize to a smaller instance size, the discounted rate does not reset. You continue to progress through the hourly discount tiers. -->

### Beispiel für eine Dauernutzung
{: #sustained-usage-example}

Angenommen, Sie kaufen eine virtuelle Serverinstanz mit der "Balanced"-Konfiguration (Ausgeglichen) mit 16 CPUs und 64 GB RAM zu einem Grundpreis von 0,795 USD pro Stunde. Im Verlauf des Monats nimmt dieser Satz wie folgt ab:

| Abgelaufene Zeit im Monat       | Abrechnungsrabatt  |  Beispielsatz     |
| ----------------------------- | ----------------- | -------- |
| 0-20%                         | regulärer Einzelhandelssatz (0,795 USD) | 116,07 USD    |                
| 21-40%                        | 5%        |   110,27 USD   |                 
| 41-60%                        | 10%       |    104,46 USD  |            
| 61-80%                        | 15%        |    98,66 USD    |                
| 81-100%                       | 20% |       92,86 USD      |
{: caption="Tabelle 2. Beispiel für gestaffelte Rabatte" caption-side="top"}  

Wenn Ihr Instanz den gesamten Monat ausgeführt wird, beträgt Ihre Gesamtrechnung bei diesem Modell 522,32 USD. Im Vergleich zum Stundensatz führen die Rabatte zu einer monatliche Gesamtersparnis von 10 %.

## Preise für Basisinstanzen
{: #base-instance-prices}

Basisinstanzpreise beginnen bei 0,087 USD pro Stunde. Wenn Sie einen virtuellen Server erstellen, werden Sie aufgefordert, eine virtuelle Serverfamilie und eine Profilkonfiguration auszuwählen. Wenn Sie Ihre Auswahl treffen, wird der zugehörige Stundensatz in der Tabelle angezeigt. <!-- You can also use the Pricing Calculator to estimate your costs. --> 

## Enthaltene Betriebssysteme
{: #included-operating-systems}

Die folgenden Betriebssysteme sind kostenlos enthalten:

* Aktuelle Version von CentOS 7.x
* Ubuntu 16.04 LTS, 18.04 (Mindestversion)
* Aktuelle Version von Debian 8.x, 9.x (Mindestversion)

Es sind Premium-Betriebssysteme und weitere Add-ons verfügbar. Die Preisstruktur wird in Ihrer Kostenübersicht angezeigt.

## Abrechnung aussetzen
{: #suspend-billing}

Wenn Sie eine Instanz ausschalten, entstehen Ihnen keine Kosten für bestimmte Rechenressourcen. Die Abrechnung wird automatisch gestoppt, wenn Sie die Instanz stoppen. Mit der Funktion zum Aussetzen der Abrechnung können Sie die Kosten reduzieren und verhindern, dass Sie eine Instanz erneut erstellen müssen, wenn Sie ihre Ressourcen erneut benötigen.

In Situationen, in denen Sie Ihre Infrastruktur in Abhängigkeit von den Workloadanforderungen nach oben und unten skalieren möchten, können Sie diese Funktion als schnellere Alternative zum Erstellen und Löschen von Instanzen verwenden.
{:tip}

### Abrechnungsdetails
{: #billing-details}

Es ist wichtig zu verstehen, welche Kosen beim Ausschalten Ihrer virtuellen Serverinstanz weiterhin anfallen bzw. nicht mehr anfallen.

Die Abrechnung wird nur ausgesetzt, wenn Sie Ihre virtuelle Serverinstanz über die IBM Cloud-Konsole, die CLI oder die API ausschalten. Wenn Sie die virtuelle Serverinstanz direkt über das Betriebssystem ausschalten, wird die Abrechnung für diese Instanz nicht ausgesetzt.
{:note}

In der folgenden Tabelle finden Sie Details darüber, wie sich das Aussetzen der Abrechnung auf verschiedene Ressourcengebühren auswirkt.

| Ressource                      | Keine Abrechnung   | Abrechnung bleibt bestehen |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| Bandbreitenupgrades            |          X        |                  |
| Betriebssystemlizenzen     |          X        |                  |
| Variable IPs, Lastausgleich und weitere angehängte Netzangebote |                   |         X        |
| Speicher                       |                   |         X        |
{: caption="Tabelle 1. Details zur Ressourcenabrechnung" caption-side="top"}   

Die Verwendungsdauer wird nach Sekunden berechnet, aufgegliedert nach Zeiten, in denen die virtuellen Serverinstanzen in Gebrauch sind, und nach Aussetzzeiten der Instanzen. Wenn Sie die Funktion zum Aussetzen der Abrechnung niemals durch das Abschalten der Instanz initiieren, wird die Abrechnung pro Sekunde des Lebenszyklus der Instanz durchgeführt. 
{:note}

#### Abrechnungs- und Dauernutzungsrabatte aussetzen
{: #suspend-billing-and-sustained-usage-discounts}

Für Dauernutzungsrabatte setzt ein ausgesetztes Image an der Stelle wieder ein, an der es bei der Rabattierung aufgehört hat. Mit anderen Worten: Ein Nutzungsrabatt gilt nur für den Zeitraum, in dem das Image im Gebrauch ist, und nicht für den Zeitraum, in dem das Image ausgesetzt wurde.

#### Mindestnutzungsgebühr
{: #minimum-usage-charge}

Für virtuelle Serverinstanzen gilt eine monatliche Mindestnutzungsgebühr. Wenn die Nutzung im Rechnungsstellungszyklus bei mehr als 25 % liegt, wird Ihnen die tatsächliche Nutzungszeit in Rechnung gestellt. Wenn die Nutzung weniger als 25 % der Zeit im Rechnungsstellungszyklus einnimmt, fällt die Mindestgebühr von 25 % an. 

Angenommen, Sie verfügen beispielsweise über eine Instanz, die über den gesamten Rechnungsstellungszyklus (720 Stunden) aktiv ist. In dieser Zeit wurde die Instanz für 577 Stunden ausgesetzt und für 143 Stunden ausgeführt. Die Instanz wird mit 180 Stunden in Rechnung gestellt (das Minimum der verfügbaren Stunden in diesem Abrechnungszeitraum).  

Sie haben jedoch auch eine Instanz, die innerhalb eines Rechnungsstellungszyklus von insgesamt 400 Stunden sowohl erstellt als auch gestoppt wurde. In dieser Zeit wurde die Instanz für 120 Stunden ausgesetzt und für 280 Stunden ausgeführt. Die Instanz würde Ihnen für ihre Nutzung von 280 Stunden in Rechnung gestellt.

#### Abrechnungsbeleg
{: #billing-invoice}

Wenn Sie die Abrechnung für einen virtuellen Server aussetzen, sehen Sie einige Änderungen in Ihrem Abrechnungsbeleg. Die relevanten Gebühren werden jetzt als nutzungsbasierte Details angezeigt. Beispielsweise werden die folgenden Zusätze angezeigt, die die verfügbaren Stunden, die genutzten Stunden und die Gesamtanzahl der in Rechnung gestellten Stunden widerspiegeln:

```
Computing instance usage...
RAM usage...
Operating system usage...
```
{:screen}

### Ressourcendetails
{: #resource-details}

#### Speicher
{: #suspend-billing-storage}

Wenn Sie die Rechnungsstellung für eine virtuelle Serverinstanz aussetzen, bleibt der zugeordnete Speicher bestehen, aber Sie können nicht auf Daten zugreifen, während die Instanz des virtuellen Servers ausgeschaltet ist. Wenn Sie die Rechnungsstellung für die Instanz wieder aufnehmen, können Sie dann auf Ihre Daten zugreifen und die normale Abrechnung wird fortgesetzt.

#### IP-Adressen
{: #suspend-billing-ip-addresses}

Alle Netzkonfigurationen und IPs (private IPs aus dem Teilnetzbereich) bleiben unverändert, während die Instanz ausgesetzt wird.

Auf der Seite mit den Instanzdetails können Sie sehen, ob Ihre Einheit gestoppt wurde. Um den Zeitpunkt der Statusänderung anzuzeigen, klicken Sie im Navigationsbereich auf **Aktivität**. 

#### Einschränkungen
{: #suspend-billing-limitations}

Ausgesetzte virtuelle Server werden weiterhin Ihrem kontoweiten Einheitenkontingent angerechnet.
