---
copyright:
  years: 2017, 2019
lastupdated: "2019-05-29"
keywords: features, benefits, isolation, provisioning, security, cloud-native, workloads, BYOIP, vpc

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Informationen zu Virtual Private Cloud
{: #about}

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) ist die IBM Cloud-Plattform der nächsten Generation. Mit VPC können Sie Ihren eigenen Speicherplatz in der IBM Cloud erstellen, um eine isolierte Umgebung innerhalb der öffentlichen Cloud auszuführen. VPC kombiniert die Sicherheit einer privaten Cloud mit der Agilität und einfachen Handhabung einer öffentlichen Cloud.

Sie können Netzservices verwalten und Instanzen nach Bedarf starten, um Ihre geschäftskritischen, cloudtoleranten und cloudbasierten Anwendungen zu unterstützen. Folgende zentrale Features stehen zur Verfügung:

* Isolierte Anwendungsumgebungen über eine API erstellen und verwalten
* Eigene Netzwertrichtlinien definieren, die für optimale Sicherheit und benutzerfreundlichen Zugriff konzipiert sind
* Netztopologien mit BYOIP (Bring Your Own IP) gestalten
* Ressourcen bereitstellen und miteinander verbinden oder voneinander isolieren
* Mehrere Regionen für Disaster-Recovery und Ausfallsicherheit abdecken
* Verfügbarkeitszonen verwenden, die schnelle Verbindungen mit hoher Verfügbarkeit und niedriger Latenz über verschiedene Regionen hinweg ermöglichen
* Hochgeschwindigkeitsnetze und schnelle Speichereinheiten verwenden
* Always-Services ermöglichen (Steuerebene)
* Kernservices bereitstellen und verwenden: IPAM, VPN, Firewalls, SSH, DNS und L4-Lastausgleich (Layer 4)
* Weitere Services konfigurieren: automatische Skalierung, CDNs, NFV von Drittanbietern

In der folgenden Abbildung mit einer Übersicht über VPC werden die verfügbaren VPC-Komponenten dargestellt und erklärt, wie durch ihr Zusammenspiel der Service und die Konnektivität bereitgestellt werden, die Sie möglicherweise benötigen. Ziehen Sie diese Abbildung wieder zu Rate, wenn Sie Ihr Wissen in Abschnitten mit Details zu einer bestimmten Komponente vertiefen.

![Übersicht über IBM Cloud VPC](images/vpc-experience-simple.svg "Übersicht über IBM Cloud VPC")

In den folgenden Abschnitten erhalten Sie eine Übersicht über die in IBM Cloud VPC verfügbaren Funktionen.

## Leistungsspektrum für den Netzbetrieb

{{site.data.keyword.cloud_notm}} VPC bietet ein einfaches und umfassendes Leistungsspektrum für den Netzbetrieb, einschließlich der Erstellung von virtuellen privaten Clouds in [global verfügbaren Regionen aus mehreren Zonen](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region), Teilnetze in unterschiedlichen Zonen, die [Auswahl des IP-Adressbereichs](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets), Virtuelle Firewalls, ([Sicherheitsgruppen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)  und [Netz-ACLs](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls)), virtuelle private Netze zwischen zwei Sites ([VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)) und Lastausgleich ([LBaaS](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)) mit Elastizität.

Ein VPC ist durch die Verwendung einer Reihe privater IP-Adressen in Teilnetze unterteilt. Standardmäßig können jedoch alle Ressourcen (zum Beispiel virtuelle Serverinstanzen) in derselben VPC unabhängig von ihrem Teilnetz miteinander kommunizieren. Teilnetze sind in einer einzigen Zone enthalten und können nicht mehrere Zonen umfassen, was bei der Sicherheit, bei der Verringerung der Latenzzeit und mit hoher Verfügbarkeit hilfreich ist.

Mit den vorgeschlagenen Präfixbereichen und vorkonfigurierten Netzsicherheitsrichtlinien können Sie ohne großen Aufwand mehrere VPCs und Teilnetze erstellen; alternativ können Sie eigene Adressenpräfixe und angepasste Sicherheitsrichtlinien entwerfen und definieren.

Die CIDR-Blöcke 161.26/16 und 166.8/14 sind reserviert und werden in jedes Teilnetz weitergeleitet. Informationen hierzu finden Sie unter [Für IBM Cloud VPC verfügbare Serviceendpunkte](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc). Außerdem ist der CIDR-Block 10.240/13 für VPC-Adresspräfixe reserviert und [auf eine vordefinierte Art](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#ibm-cloud-vpc-and-address-prefixes) in Zonen aufgeteilt und kann nicht geändert werden.
{: important}

### Internetzugriff

Es stehen drei Optionen zur Verfügung, mit denen die Kommunikation zwischen den virtuellen Serverinstanzen (VSIs) und dem öffentlichen Internet ermöglicht wird:
* Verwenden Sie ein öffentliches Gateway (PGW, Public Gateway), um die Kommunikation im Internet für alle Instanzen des virtuellen Servers im verbundenen Teilnetz zu aktivieren. Es fällt außer für die verwendete Bandbreite keine Gebühr für die Verwendung eines PGW an.
* Verwenden Sie eine variable IP-Adresse (FIP, Floating-IP), um die Kommunikation zu und von einer einzelnen virtuellen Serverinstanz (VSI) ins Internet zu ermöglichen.
* Verwenden Sie ein VPN-Gateway. Weitere Informationen finden Sie in der [Dokumentation zu 'VPN für VPC'](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc#--using-vpn-with-your-vpc).
{: note}

### Klassischer Zugriff

Vorhandene {{site.data.keyword.cloud_notm}}-Infrastrukturbenutzer können ihre klassische Infrastruktur, einschließlich der Direct Link-Konnektivität, mit einer VPC pro Region über den [klassischen Zugriff](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc) verbinden.

### BYOIP

Sie können mit BYOIP (Bring your own IP) Ihren eigenen öffentlichen IPv4-Adressbereich in Ihr IBM Cloud VPC-Konto einbringen.

Wenn Sie BYOIP verwenden, muss {{site.data.keyword.cloud_notm}} diese IPv4-Adressen für {{site.data.keyword.cloud_notm}}-Ressourcen konfigurieren, die Pakete an diese und von diesen Adressen senden, die von Ihnen bereitgestellt werden. Daher können diese IP-Adressen als Ergebnis der Verwendung des bereitgestellten IPv4-Bereichs in {{site.data.keyword.cloud_notm}} den IBM Support-Mitarbeitern und Dritten bereitgestellt werden.
{: important}

Sie müssen die API oder die Befehlszeilenschnittstelle (CLI) verwenden, um BYOIP nutzen zu können. Diese Funktionalität ist über die Benutzerschnittstelle der IBM Cloud-Konsole nicht verfügbar.
{: note}

### Globale Konnektivität

Sie können Ihre Anwendungen und die verfügbaren Ressourcen auf mehrere Regionen verteilt verwenden. Über ein [VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc) können Sie private Verbindungen zu anderen Projekten und Teilen Ihrer Hybrid-Cloudbereitstellung erstellen.

Einfache Skalierung und gemeinsame Nutzung; ein VPC-Netz und die zugehörigen Ressourcen können sich von Ihrer On-Premises-Einrichtung bis in die Cloud erstrecken.

## Sicherheit

Die Sicherheit ist in Form von [Sicherheitsgruppen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups), die als virtuelle Firewalls für den Schutz auf Instanzebene dienen, und [Netzzugriffssteuerungslisten](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) (Netz-ACLs) für den Schutz auf Teilnetzebene in {{site.data.keyword.cloud_notm}} VPC integriert.

## Hochverfügbarkeit

Mit {{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) können Sie Anwendungen logisch von anderen Netzen in allen Regionen isolieren und gleichzeitig Skalierbarkeit und Sicherheit gewährleisten. Verwenden Sie [Lastausgleichsfunktionen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc), um den Netzverkehr zur Verbesserung der Leistung und Hochverfügbarkeit (HA, High Availability) auf eine Reihe von Zielen zu verteilen. Lastausgleichsfunktionen überwachen zudem den Allgemeinzustand Ihrer Anwendungen und Services. Sie können eine Lastausgleichsfunktion einrichten, um den eingehenden Anwendungsdatenverkehr über Instanzen in einer einzigen Zone oder über mehrere Zonen in einer Region zu verteilen.

## Rechenfunktionen

Verwenden Sie [IBM Cloud Virtual Servers for Virtual Private Cloud](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-virtual-private-cloud) zum Bereitstellen skalierbarer Rechenressourcen in IBM Cloud. Erstellen Sie virtuelle Serverinstanzen (VSIs) schnell mithilfe vordefinierter [Profile](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles), die für Ihre spezifischen Workloads optimiert sind. Wenn Instanzen, die [über mehrere virtuelle NICs verfügen](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-network-security-options) und mit mehreren Netzen verbunden sind, bereitgestellt werden, wählen Sie eines der unterstützten [Archivbilder](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-images) aus oder laden Sie ein angepasstes Bild hoch. 

Von {{site.data.keyword.cloud_notm}} VPC wird eine Ebene zur Netzorchestrierung hinzugefügt, von der die Podgrenze der virtuellen Serverinstanzen beseitigt wird und somit eine unbegrenzte Kapazität zum Skalieren der Instanzen ermöglicht wird. Von der Ebene für die Netzorchestrierung wird der gesamte Netzbetrieb für alle virtuellen Serverinstanzen verarbeitet, die sich bei Verwendung von IBM Cloud VPC in den Regionen und Zonen befinden. Mit den softwaredefinierten Netzfunktionen, die von {{site.data.keyword.cloud_notm}} VPC bereitgestellt werden, verfügen Sie über mehr Optionen für VPNs, LBaaS-Instanzen, Instanzen mit mehreren virtuellen Netzschnittstellenkarten und größere Teilnetzgrößen.

## Speicherleistungsmerkmale

Wenn Sie eine {{site.data.keyword.cloud_notm}} Virtual Servers for Virtual Private Cloud-Instanz bereitstellen, wird automatisch ein 100 GB großer Blockspeicherdatenträger für allgemeine IOPS-Workloads (3 IOPS/GB) als primärer Bootdatenträger erstellt und der Instanz zugeordnet. Sie können Blockspeicherdatenträger erstellen, wenn Sie eine Instanz des virtuellen Servers in einem VPC-Netz bereitstellen, oder neue Datenträger erstellen, die unabhängig vom VSI-Lebenszyklus sind.

Wenn Sie zusätzlichen Blockspeicher für die VPC bereitstellen, können Sie ein [IOPS-Tier](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#tiers) für den Blockspeicherdatenträger auswählen oder ein [angepasstes IOPS-Profil](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#custom) angeben.

## Für cloudortientierte und hybride Workloads konzipiert

VPC ist ideal für cloudorientierte Workloads und für die Verknüpfung der vorhandenen Infrastruktur mit {{site.data.keyword.cloud_notm}}. Sie können die beste Cloud für Workloads, wie Cognitive Computing, AI und Machine Learning, erstellen. 

Nachfolgend finden Sie weitere Beispiele dafür, wie VPC hybride, cloudtolerante und cloudorientierte Hybrid-Workloads unterstützt:

* Lastausgleich mit horizontaler automatischer Skalierung
* Überwachung und Statusprüfung
* Unterstützung für GPUs
* VSI-Bereitstellung mit Affinitäts- und Anti-Affinitäts-Gruppen

## Weitere Informationen

* [**VPC-Terminologie**](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-glossary)
* [**VPC-Sicherheit**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-security-in-your-ibm-cloud-vpc#security-in-your-ibm-cloud-vpc)
* [**Verfügbare Regionen und Teilnetze für VPC**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)
* [**Netzressourcen in VPC erstellen und verwalten**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-network-resources-in-vpc)
* [**Virtuelle Serverinstanzen in VPC erstellen und verwalten**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-virtual-server-instances)
* [**Blockspeicher in VPC erstellen und verwalten**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-storage-in-vpc)
