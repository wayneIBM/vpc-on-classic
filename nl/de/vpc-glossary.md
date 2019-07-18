---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: glossary, terminology, definition, access, floating, geography, image, region, zone, instance, VSI, LBaaS, VPN, VPC, NAT, profile, resource, security group, shares, storage, VNPaaS, volume, subnet, SSH, key, gateway, ACL

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

# VPC-Glossar
{: #vpc-glossary}

Die folgende Terminologie wird häufig verwendet und ist spezifisch für {{site.data.keyword.cloud}} Virtual Private Cloud. Weitere Erläuterungen und Begriffe finden Sie im Abschnitt mit den [Glossareinträgen für IBM Cloud](/docs/overview/glossary?topic=overview-glossary).

## Datenträger
{: #volumes}

An einen Hypervisor angehängter, leistungsfähiger Datenspeicher für virtuelle Serverinstanzen in einer VPC.

## Gemeinsam genutzte Ressourcen
{: #shares}

Eine Möglichkeit, Dateispeicher in einer VPC bereitzustellen.

## Geografie
{: #geography}

In einer VPC stellt die geografische Bezeichnung Informationen zu der Region und Zone bereit, in denen die VPC und die zugehörigen Ressourcen implementiert werden.

## Image
{: #image}

Die Informationen, die zum Starten einer virtuellen Serverinstanz (oder _Instanz_) erforderlich sind. In der Regel handelt es sich um ein Momentaufnahmeimage eines im Handel erhältlichen Betriebssystems, das zum Booten verwendet wird.

## Impliziter Router
{: #implicit-router}

Die inhärente Netzkonnektivität zwischen allen Teilnetzen, die innerhalb einer VPC erstellt wurden.

## Instanz
{: #instance}

Ein Kurzname für einen virtuellen Server (VSI), der in einer VPC ausgeführt wird.

## LBaaS
{: #lbaas}

Load Balancer as a Service (LBaaS) bietet die Möglichkeit, den Datenverkehr in einer ausgewogenen Zuordnung zwischen Instanzen in einer VPC zu verteilen.

## NAT
{: #nat}

Network Address Translation (NAT) ist eine im Rahmen des [Internetstandards RFC 1631 ![Symbol für externen Linnk](../../icons/launch-glyph.svg "Symbol für externen Link")](https://tools.ietf.org/html/rfc1631){: new_window} beschriebene Adressumsetzung, über die eine IP-Adresse mit mehreren anderen IP-Adressen, z. B. in einem privaten Teilnetz, im Wesentlichen über eine Lookup-Tabelle kommunizieren kann. Es gibt zwei NAT-Haupttypen: 1:1 NAT (1:1-Umsetzung) und [n:1 NAT (n:1-Umsetzung) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Network_address_translation){: new_window}. NAT war ursprünglich als eine Möglichkeit gedacht, die Nutzungsdauer von IPv4-IP-Adressen zu verlängern, wird jetzt aber in der Regel in Cloudumgebungen verwendet, um die Kommunikation mit privaten Teilnetzen aufzubauen.

## Öffentliches Gateway
{: #public-gateway}

Ein öffentliches Gateway (Public Gateway, PGW) ermöglicht ausschließlich abgehenden Zugriff für ein Teilnetz (mit allen VSIs, die an das Teilnetz angeschlossen sind), um eine Verbindung zum Internet herzustellen. Beachten Sie, dass Teilnetze standardmäßig privat sind; optional können Sie jedoch ein öffentliches Gateway erstellen und ihm ein Teilnetz zuordnen. Nach dieser Zuordnung können alle VSIs in diesem Teilnetz eine Verbindung zum Internet herstellen.

Ein öffentliches Gateway verwendet n:1-NAT, was bedeutet, dass Tausende VSIs mit privaten Adressen eine öffentliche IP-Adresse für die Kommunikation mit dem öffentlichen Internet verwenden. Das öffentliche Gateway ermöglicht es dem Internet nicht, eine Verbindung zu diesen Instanzen aufzubauen.

## Profil
{: #profile}

Ein Profil ist eine gängige Kombination aus vCPU und RAM, die schnell instanziiert werden kann, um eine virtuelle Serverinstanz (VSI) zu starten. Es werden drei Profilfamilien unterstützt: 'Balanced', 'Compute' und 'Memory'.


## Region
{: #region}

Ein geographischer Bereich, in dem eine VPC bereitgestellt wird. Jede Region enthält mehrere Zonen, die unabhängige Fehlerbereiche darstellen. IBM Cloud VPC umfasst mehrere Zonen in der zugeordneten Region.

## Ressource
{: #resource}

Eine Art von Asset, das Teil einer VPC-Bereitstellung ist und für das Abrechnungsgebühren anfallen können. Beispiele für eine Ressource sind eine VSI, eine variable IP-Adresse oder die VPC selbst. Ressourcen können in _Ressourcengruppen_ zusammengefasst werden, um die Abrechnung einfacher zu machen, wenn bestimmte Ressourcen immer in einer Kombination in einer VPC verwendet werden.

## Sicherheitsgruppe
{: #security-group}

Eine Sicherheitsgruppe fungiert als virtuelle Firewall, die den eingehenden und abgehenden Datenverkehr für einen oder mehrere Server (VSIs) steuert. Eine Sicherheitsgruppe ist eine Gruppe von Regeln, die angeben, ob Datenverkehr für eine zugeordnete VSI zugelassen werden soll. Wenn ein Kunde eine VSI startet, kann er eine oder mehrere Sicherheitsgruppen mit dieser VSI verknüpfen. Mit den entsprechenden Berechtigungen können Kunden Sicherheitsgruppenregeln ändern.

## SSH-Schlüssel
{: #ssh-key}

Automatisch generierte, verschlüsselte Zugriffsberechtigungsnachweise, die den Zugriff auf virtuelle Serverinstanzen in einer VPC steuern.

## Teilnetz
{: #subnet}

Ein Teilnetz ist ein IP-Adressbereich, der an eine einzelne Zone gebunden ist und sich nicht über mehrere Zonen oder Regionen erstrecken kann. Ein Teilnetz kann die Gesamtheit einer Zone in einer IBM Cloud VPC-Instanz umfassen.

Im Rahmen von IBM Cloud VPC ist die für ein Teilnetz wichtige Eigenschaft die Tatsache, dass Teilnetze voneinander isoliert werden und in üblicher Weise miteinander verbunden werden können. Die Isolation von Teilnetzen kann durch Netzzugriffssteuerungslisten (Access Control Lists, ACLs) erreicht werden, die als Firewalls zum Steuern des Datenflusses von Datenpaketen zwischen Teilnetzen fungiert. In ähnlicher Weise agieren Sicherheitsgruppen als virtuelle Firewalls, um den Datenfluss von Datenpaketen zu und von einzelnen virtuellen Serverinstanzen (VSIs) zu steuern.

Es ist die Isolation von Teilnetzen, die es Ihnen ermöglicht, einen privaten Bereich innerhalb der öffentlichen Cloud zu erstellen.

## Variable IP-Adresse
{: #floating-ip}

Mit variablen IP-Adressen können Sie eingehenden und abgehenden Internetzugriff für VPC-Ressourcen, wie Instanzen, eine Lastausgleichsfunktion oder einen VPN-Tunnel, über zugeordnete _variable IP-Adressen_ aus einem Pool bereitstellen.

Variable IP-Adressen sind öffentliche IP-Adressen, die vom System aus dem bereits vorhandenen Pool bereitgestellt werden. Es ist nicht möglich, eigene öffentliche IP-Adressen zu verwenden. Variable IP-Adressen sind über das Internet zugänglich und können einer Instanz (z. B. einer virtuellen Serverinstanz, einer Lastausgleichsfunktion oder einem VPN-Gateway) über einen vNIC zugeordnet werden.

Sie können eine variable IP-Adresse aus dem Pool der verfügbaren variablen IP-Adressen, die von IBM bereitgestellt werden, reservieren und sie einer beliebigen Instanz in derselben VPC zuordnen oder diese Zuordnung aufheben. Variable IP-Adressen nutzen 1:1 [NAT](#nat), damit ein Server mit dem öffentlichen Internet und mit einem privaten Teilnetz in Ihrer Cloudumgebung kommunizieren kann.

## VPC
{: #vpc}

An ein Konto gebundenes virtuelles Netz. Es bietet eine differenzierte Kontrolle über die virtuelle Infrastruktur und die Segmentierung des Netzes, zusammen mit der Sicherheit und der Möglichkeit einer dynamischen Skalierung.

## VPN
{: #vpn}

Ein virtuelles privates Netz (VPN) ermöglicht eine private Verbindung zwischen zwei Endpunkten, selbst wenn die Daten über ein öffentliches Netz übertragen werden. Kunden können Daten gemeinsam nutzen, als wären sie mit einem privaten Netz verbunden. In der Regel wird ein VPN in Kombination mit Sicherheitsmethoden wie Authentifizierung und Verschlüsselung verwendet, um maximale Datensicherheit und Privatsphäre zu gewährleisten.

## VPNaaS
{: #vpnaas}

'VPN as a Service' (VPNaaS) bietet die Möglichkeit, ein VPN mit Endpunkten innerhalb und außerhalb einer VPC einzurichten.

## VPN-Verbindung
{: #vpn-connection}

Eine private Verbindung zwischen zwei Endpunkten, die privat bleibt und auch dann verschlüsselt werden kann, wenn die Daten über ein öffentliches Netz übertragen werden.

## Zone
{: #zone}

Ein unabhängiger Fehlerbereich. Eine Zone ist eine Abstraktion, die zur Unterstützung bei verbesserter Fehlertoleranz und verringerter Latenzzeit konzipiert ist. Eine Zone garantiert die folgenden Eigenschaften:

 * Da es sich bei jeder Zone um einen unabhängigen Fehlerbereich handelt, ist es äußerst unwahrscheinlich, dass zwei Zonen in einer Region gleichzeitig fehlschlagen.
 * Für den Datenverkehr zwischen den Zonen in einer Region liegt die Latenz bei weniger als 2 ms.
## Zugriffssteuerungsliste (Access Control List, ACL)
{: #access-control-list}

Eine Zugriffssteuerungsliste (Access Control List, ACL) stellt Sicherheitskontrollen für den eingehenden und abgehenden Datenverkehr für ein [Teilnetz](#subnet) bereit. Eine ACL ist statusunabhängig. Jede ACL setzt sich aus Regeln zusammen, die auf einer _Quellen-IP_, einem _Quellenport_, einer _Ziel-IP_, einem _Zielport_ und einem _Protokoll_ basieren.

In IBM Cloud VPC wird jedes Teilnetz mit einer Standard-ACL erstellt, die eingehenden und abgehenden Datenverkehr ermöglicht; Kunden können jedoch auch angepasste ACLs erstellen. Es ist immer nur eine ACL an ein Teilnetz angeschlossen, aber eine ACL kann mehreren Teilnetzen zugeordnet werden.

Wird manchmal auch als _Netz-ACL_ oder NACL bezeichnet.

