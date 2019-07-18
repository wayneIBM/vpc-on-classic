---
copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: vpc, classic, access, API, CLI, limitations

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

# Zugriff auf die klassische Infrastruktur über eine VPC einrichten
{: #setting-up-access-to-your-classic-infrastructure-from-vpc}
[comment]: # (Verknüpfter Hilfeabschnitt)

Sie können für jedes beliebige Konto die klassische {{site.data.keyword.cloud}}-Infrastruktur, einschließlich Direct Link-Konnektivität, über eine VPC in jeder Region einrichten. Von diesen speziellen "VPCs mit klassischem Zugriff" wird dieselbe Routing-Funktion (impliziter Router) wie die klassische {{site.data.keyword.cloud}}-Infrastruktur verwendet. Es wird erwartet, dass die Leser mit dem Netzbetrieb der klassischen Infrastruktur vertraut sind.

Wenn Sie eine VPC-Instanz für den klassischen Zugriff einrichten, kann jeder Rechenhost (VSI- oder Bare-Metal-Host) ohne allgemein zugängliche Schnittstelle in einem klassischen Konto Pakete über die VPC mit klassischem Zugriff senden und empfangen. Denken Sie jedoch daran, dass Firewalls, Gateways, Netz-ACLs oder Sicherheitsgruppen diesen Datenverkehr filtern können. Es wird empfohlen, dass nur der Datenverkehr zugelassen wird, der für die ordnungsgemäße Funktion Ihrer Anwendungen erforderlich ist.

Für Hosts mit einer allgemein zugänglichen Schnittstelle müssen Sie die Weiterleitung zurück zu der VPC-Instanz hinzufügen, die für den klassische Zugriff aktiviert ist.
{: important}

## Voraussetzungen:
{: #vpc-prerequisites}

1. Ihr klassisches Konto muss mit Ihrem IBM Cloud-Konto verknüpft sein. Entsprechende Anweisungen dazu finden Sie im Abschnitt zum [Verknüpfen von IBMid-Konten](/docs/account?topic=account-unifyingaccounts).
1. Ihr klassisches Konto muss für VRF aktiviert sein.
    * Wenn Sie bereits über Direct Link für Ihr Konto verfügen, sind Sie bereit.
    * Wenn Ihr Konto nicht VRF-fähig ist, öffnen Sie ein Ticket, um "VRF-Migration" für Ihr Konto zu beantragen. Weitere Informationen zu diesem Konvertierungsprozess finden Sie im Abschnitt zur [Vorgehensweise bei der Initiierung der Konvertierung](/docs/infrastructure/direct-link?topic=direct-link-how-you-can-initiate-the-conversion#how-you-can-initiate-the-conversion) in der Dokumentation zu Direct Link.

Firewalls, Gateways, Netz-ACLs oder Sicherheitsgruppen können einen Teil oder den gesamten Netzwerkverkehr zwischen klassischen und VPC-Ressourcen herausfiltern.
{: important}

Alle Teilnetze in einer VPC mit klassischem Zugriff werden gemeinsam in VRF für die klassische Infrastruktur genutzt; die IP-Adressen werden somit im Bereich `10.0.0.0/8` verwendet. Wenn Sie Konflikte mit IP-Adressen vermeiden möchten, **verwenden Sie nicht** IP-Adressen im Bereich `10.0.0.0/8`, wenn Sie Teilnetze in einer VPC mit klassischem Zugriff erstellen.
{: important}

## VPC mit klassischem Zugriff erstellen
{: #create-a-classic-access-vpc}

Sie können eine VPC mit klassischem Zugriff über die Benutzerschnittstelle (UI), die Befehlszeilenschnittstelle (CLI) oder die Anwendungsprogrammierschnittstelle (API) erstellen.

Eine VPC muss bei ihrer Erstellung für den klassischen Zugriff konfiguriert werden: Sie können eine VPC nicht so aktualisieren, dass der klassische Zugriff hinzugefügt oder entfernt wird.
{: note}

### VPC mit klassischem Zugriff über die Benutzerschnittstelle erstellen
{: #create-a-classic-access-vpc-from-the-user-interface}

Erstellen Sie eine VPC mit klassischem Zugriff über die Seite **VPC erstellen**, indem Sie das Kontrollkästchen **Zugriff auf klassische Ressource aktivieren** unter der Beschriftung **Klassischer Zugriff** auswählen.

![UI für klassischen Zugriff](/images/classic-access-ui.png)

### VPC mit klassischem Zugriff über die Befehlszeilenschnittstelle erstellen
{: #create-a-classic-access-vpc-using-the-cli}

Verwenden Sie das Flag `--classic-access` beim Erstellen der VPC. Zum Beispiel:

```
ibmcloud is vpc-create my-access-vpc --classic-access
```
{: pre}


### VPC mit klassischem Zugriff über die API erstellen
{: #create-a-classic-access-vpc-using-the-api}

Übergeben Sie den Parameter `classic_access` beim Erstellen der VPC. Zum Beispiel:

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=2019-05-31&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### Standardadresspräfixe in einer VPC mit klassischem Zugriff
{: #classic-access-default-address-prefixes}

Alle Teilnetze in einer VPC mit klassischem Zugriff werden gemeinsam von der VRF-Funktion (VRF - Virtual Routing and Forwarding) für die klassische Infrastruktur genutzt; die IP-Adressen werden somit im Bereich `10.0.0.0/8` verwendet. Wenn Sie Konflikte mit IP-Adressen vermeiden möchten, **verwenden Sie nicht** IP-Adressen im Bereich `10.0.0.0/8`, wenn Sie Teilnetze in einer VPC mit klassischem Zugriff erstellen.
{: important}

Wenn eine neue VPC mit klassischem Zugriff erstellt wird, werden die Standardadresspräfixe wie in der folgenden Tabelle dargestellt auf Basis der Region und Zone zugewiesen:

Zone         | Adresspräfix  
---------------|---------------
`us-south-1`   | `172.16.0.0/18`
`us-south-2`   | `172.16.64.0/18`
`us-south-3`   | `172.16.128.0/18`
`us-east-1`    | `172.17.0.0/18`
`us-east-2`    | `172.17.64.0/18`
`us-east-3`    | `172.17.128.0/18`
`eu-gb-1`      | `172.18.0.0/18`
`eu-gb-2`      | `172.18.64.0/18`
`eu-gb-3`      | `172.18.128.0/18`
`eu-de-1`      | `172.19.0.0/18`
`eu-de-2`      | `172.19.64.0/18`
`eu-de-3`      | `172.19.128.0/18`
`jp-tok-1`     | `172.20.0.0/18`
`jp-tok-2`     | `172.20.64.0/18`
`jp-tok-3`     | `172.20.128.0/18`
`au-syd-1`     | `172.21.0.0/18`
`au-syd-2`     | `172.21.64.0/18`
`au-syd-3`     | `172.21.128.0/18`


## Einschränkungen
{: #vpc-limitations}

* Mit dem impliziten privaten Router des Kontos werden nur private Netze (in der alten Dokumentation als 'Back' bezeichnet) verbunden. 
* Nur Teilnetze, die mit klassischen APIs zugeordnet sind, werden mit dem klassischen Teil Ihres impliziten privaten Routers verbunden. Die Weiterleitungsfunktion des impliziten Routers funktioniert nicht zwischen Teilnetzen in klassischen VLANs. 
* Es kann nur eine VPC pro Region und pro Konto für den klassischen Zugriff aktiviert werden.

Die Konfigurationsprozedur variiert abhängig von dem Betriebssystem, das Sie auf den klassischen VSIs oder Bare-Metal-Servern installiert haben. Weitere Informationen finden Sie unter [Informationen zu öffentlichen virtuellen Servern](https://cloud.ibm.com/docs/vsi?topic=virtual-servers-about-public-virtual-servers).
{: note}
