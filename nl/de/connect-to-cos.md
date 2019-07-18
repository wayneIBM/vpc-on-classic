---
copyright:
  years: 2018-2019
lastupdated: "2019-06-11"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, datacenter

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
{:DomainName: data-hd-keyref="DomainName"}

# Verbindung von VPC zu IBM Cloud Object Storage herstellen
{: #connecting-to-ibm-cloud-object-storage-from-a-vpc}

In diesem Dokument erfahren Sie, wie Sie von VPC eine Verbindung zu {{site.data.keyword.cloud}} Object Storage herstellen. Die Endpunktinformationen werden in einer Umgebung mit klassischer Infrastruktur gezielt auf {{site.data.keyword.cloud_notm}} Virtual Private Cloud angewendet.
{: shortdesc}


## Was ist IBM Cloud Object Storage (COS)?
{: #what-is-ibm-cloud-object-storage-cos}

{{site.data.keyword.cloud}} Object Storage (COS) ist eine webbasierte Plattform zum Speichern unstrukturierter Daten. Diese Plattform bietet Zuverlässigkeit, Sicherheit, Verfügbarkeit und Disaster-Recovery ohne Replikation.

Mit {{site.data.keyword.cloud_notm}} Object Storage gespeicherte Daten werden verschlüsselt und auf mehrere geografische Standorte verteilt. Die Daten sind über eine Implementierung der S3-API zugänglich. Dieser Service nutzt Technologien für die verteilte Speicherung, die vom IBM Cloud Object Storage-System bereitgestellt werden.

IBM Cloud Object Storage wird mit drei Konfigurationstypen für Ausfallsicherheit bereitgestellt: **Regionsübergreifend**, **Regional** und **Einzelnes Rechenzentrum**. 
* Der **regionsübergreifende** Service bietet größere Dauerhaftigkeit und höhere Verfügbarkeit als die Verwendung einer einzigen Region auf Kosten einer etwas höheren Latenz. Dieser Service ist aktuell in den Regionen USA, EU und Asien/Pazifik verfügbar.  
* Der **regionale** Service bietet die umgekehrte Funktionalität. Die Objekte werden auf mehrere Verfügbarkeitszonen innerhalb einer Region verteilt. Er ist in den Regionen USA, EU und Asien/Pazifik verfügbar. Wenn eine Region oder Zone nicht zugänglich ist, kann der Objektspeicher reibungslos weiter genutzt werden. Der Service für ein **einzelnes Rechenzentrum** verteilt Objekte an mehrere Maschinen an demselben physischen Standort. Überprüfen Sie dieses Dokument regelmäßig auf die verfügbaren Regionen.

### Direkte COS-Endpunkte für die Verwendung mit VPC
{: #cos-direct-endpoints-for-use-with-vpc}

Damit Sie eine REST-API-Anforderung senden können, müssen Sie einen Zielendpunkt oder eine URL festlegen und jede COS-Speicherposition muss über eigene URLs verfügen. Von direkten Endpunkten werden den Servern sehr schnelle Direktverbindungen zu {{site.data.keyword.cloud_notm}}-Services ohne Zusatzkosten für die Bandbreite bereitgestellt. Mit einem direkten COS-Endpunkt kann eine VPC eine Verbindung zu einem COS-Bucket herstellen, das sich an einem beliebigen Standort weltweit befinden kann.  

Für den Datenverkehr von einer VPC-Instanz zu allen auf dieser Seite aufgeführten COS-Endpunkten werden keine Gebühren berechnet.
{: note}

* Ein VPC-Client verfügt über den öffentlichen Endpunkt auch über Zugriff auf das COS-Bucket.
* Ein VPC-Client verfügt nur über den öffentlichen Endpunkt über Zugriff auf die [API für die COS-Konfiguration](https://{DomainName}/apidocs/cos/cos-configuration), nicht über den direkten Endpunkt. Mit der API für die COS-Konfiguration können manche COS-Funktionen der Buckets konfiguriert sowie die Metadaten der Buckets angezeigt werden. 
* Von einem VPC-Client kann nicht auf ein COS-Bucket zugegriffen werden, wenn die Firewall aktiviert ist.

## Vorgehensweise zum Herstellen einer Verbindung zu IBM Cloud Object Storage (COS) über eine VPC
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. Stellen Sie COS über den [Katalog ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/catalog/services/cloud-object-storage){: new_window} bereit.
2. Erstellen Sie ein COS-Bucket in einer der Regionen, die im folgenden Abschnitt aufgelistet sind.
3. Verwenden Sie den Endpunkt für die Kommunikation mit dem COS-Bucket. 

### Regionale Endpunkte
{: #regional-endpoints}

Von Buckets, die an einem regionalen Endpunkt erstellt wurden, werden Daten an drei Rechenzentren verteilt, die sich in einem Ballungsraum befinden. Der Ausfall oder sogar die Zerstörung eines einzelnen Rechenzentrums beeinträchtigt nicht die Verfügbarkeit. 

| **Region** | **Endpunkt** |
|------------|-------------------------------|
| Vereinigte Staaten (Süden) | `s3.direct.us-south.cloud-object-storage.appdomain.cloud`|
| Vereinigte Staaten (Osten) | `s3.direct.us-east.cloud-object-storage.appdomain.cloud`|
| EU (Vereinigtes Königreich) | `s3.direct.eu-gb.cloud-object-storage.appdomain.cloud`|
| EU (Deutschland) | `s3.direct.eu-de.cloud-object-storage.appdomain.cloud`|
| Asien/Pazifik (Australien) | `s3.direct.au-syd.cloud-object-storage.appdomain.cloud`
| Asien/Pazifik (Japan) | `s3.direct.jp-tok.cloud-object-storage.appdomain.cloud` |


### Regionsübergreifende Endpunkte
{: #cross-region-endpoints}

Von Buckets, die an einem regionsübergreifenden Endpunkt erstellt wurden, werden Daten an drei Regionen verteilt. Ein Ausfall oder sogar die Zerstörung einer einzelnen Region beeinträchtigt nicht die Verfügbarkeit. Anforderungen werden unter Verwendung der BGP-Weiterleitung (BGP = Border Gateway Protocol) an ein Rechenzentrum der nächstgelegenen Region weitergeleitet.

Bei einem Ausfall werden Anforderungen automatisch an eine aktive Region weitergeleitet. Fortgeschrittene Benutzer, die eine eigene Failoverlogik schreiben möchten, können hierfür das Senden von Anforderungen an den Endpunkt einer bestimmten Region und das Umgehen der BGP-Weiterleitung festlegen. 

| **Regionsübergreifend USA** | **Endpunkt** |
|------------|-------------------------------|
| Regionsübergreifend USA | `s3.direct.us.cloud-object-storage.appdomain.cloud` |
| Zugriffspunkt Dallas | `s3.direct.dal.us.cloud-object-storage.appdomain.cloud` |
| Zugriffspunkt San Jose | `s3.direct.sjc.us.cloud-object-storage.appdomain.cloud` |

| **Regionsübergreifend EU** | **Endpunkt** |
|------------|-------------------------------|
| Regionsübergreifend EU | `s3.direct.eu.cloud-object-storage.appdomain.cloud` |
| Zugriffspunkt Amsterdam | `s3.direct.ams.eu.cloud-object-storage.appdomain.cloud` |
| Zugriffspunkt Frankfurt | `s3.direct.fra.eu.cloud-object-storage.appdomain.cloud` |
| Zugriffspunkt Mailand | `s3.direct.mil.eu.cloud-object-storage.appdomain.cloud` |

| **Regionsübergreifend Asien/Pazifik** | **Endpunkt** |
|------------|-------------------------------|
| Regionsübergreifend Asien/Pazifik | `s3.direct.ap.cloud-object-storage.appdomain.cloud` |
| Zugriffspunkt Tokio | `s3.direct.tok.ap.cloud-object-storage.appdomain.cloud` |
| Zugriffspunkt Seoul | `s3.direct.seo.ap.cloud-object-storage.appdomain.cloud` |
| Zugriffspunkt Hongkong | `s3.direct.hkg.ap.cloud-object-storage.appdomain.cloud` |


 ### Endpunkte in einzelnen Rechenzentren
 {: #single-datacenter-endpoints}

Einzelne Rechenzentren befinden sich nicht an demselben Standort wie IBM Cloud-Services (zum Beispiel IAM oder Key Protect) und bieten somit keine Ausfallsicherheit, wenn ein Standort ausfällt oder zerstört wird. 

Wenn in einer Partition ein Fehler im Netzbetrieb auftritt, sodass vom Rechenzentrum keine Verbindung zu einer IBM Cloud-Kernregion für den Zugriff auf IAM hergestellt werden kann, werden die Authentifizierungs- und Berechtigungsinformationen aus einem Cache gelesen, der möglicherweise nicht aktuell ist. Diese Situation kann für bis zu 24 Stunden zu einer mangelnden Durchsetzung neuer oder geänderter IAM-Richtlinien führen.
{:important}

| **Region** | **Endpunkt** |
|------------|-------------------------------|
| Amsterdam, Niederlande | `s3.direct.ams03.cloud-object-storage.appdomain.cloud` |
| Chennai, Indien | `s3.direct.che01.cloud-object-storage.appdomain.cloud` |
| Hongkong | `s3.direct.hkg02.cloud-object-storage.appdomain.cloud` |
| Melbourne, Australien | `s3.direct.mel01.cloud-object-storage.appdomain.cloud` |
| Mexiko Stadt, Mexiko | `s3.direct.mex01.cloud-object-storage.appdomain.cloud` |
| Mailand, Italien | `s3.direct.mil01.cloud-object-storage.appdomain.cloud` |
| Montréal, Kanada | `s3.direct.mon01.cloud-object-storage.appdomain.cloud` |
| Oslo, Norwegen | `s3.direct.osl01.cloud-object-storage.appdomain.cloud` |
| San Jose, USA | `s3.direct.sjc04.cloud-object-storage.appdomain.cloud` |
| São Paulo, Brasilien | `s3.direct.sao01.cloud-object-storage.appdomain.cloud` |
| Seoul, Südkorea | `s3.direct.seo01.cloud-object-storage.appdomain.cloud` |
| Toronto, Kanada | `s3.direct.tor01.cloud-object-storage.appdomain.cloud` |
