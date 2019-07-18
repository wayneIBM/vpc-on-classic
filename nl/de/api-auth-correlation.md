---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, API, CLI, editor, viewer, administrator, operator

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

# Erforderliche Ressourcenberechtigungen für API- und CLI-Aufrufe
{: #resource-authorizations-required-for-api-and-cli-calls}

In der folgenden Tabelle sind die Berechtigungen aufgelistet, die für die Interaktion mit {{site.data.keyword.cloud}} Virtual Private Cloud-Infrastrukturobjekten erforderlich sind. Diese Informationen sind besonders hilfreich, wenn Sie API-Aufrufe vornehmen. Hier ist, was Sie wissen müssen, um diese Tabelle zu verwenden:

Die Begriffe _zugeordnet_ bzw. _nicht zugeordnet_ beziehen sich darauf, ob die Ressource einer VPC oder VPCs zugeordnet ist (entweder direkt oder indirekt über die Teilnetze der Ressource). Eine nicht zugeordnete variable IP-Adresse oder Netzzugriffssteuerungsliste (ACL) verfügt über keine Teilnetze und ist daher keiner VPC zugeordnet, sodass die Autorisierung durch VPC nicht maßgeblich ist.

* Aktionen des Typs **Anzeigen** und **Auflisten** entsprechen der Rolle **Anzeigeberechtigter**.
* Aktionen des Typs **Operation ausführen** entsprechen der Rolle **Operator**.
* Aktionen des Typs **Aktualisieren** und zugehörige Aktionen entsprechen der Rolle **Editor** oder **Administrator**.
* Ressourcen sind durch entsprechende Berechtigungen geschützt, Referenzobjekte für Ressourcen jedoch nicht (ID, CRN usw.).


| Ressource | Operation | Erforderliche Berechtigung |
|--------|--------|---------|
| VPC | Erstellen | Berechtigung zum Anzeigen für die Ressourcengruppe für diese VPC und Berechtigung zum Aktualisieren für VPC-Ressourcen|
| VPC | Aktualisieren, Löschen |  Berechtigung zum Aktualisieren für VPC |
| VPC | Anzeigen, Auflisten | Berechtigung zum Anzeigen für VPC  |
| Standard-ACL für VPC, Standard-SG|  Anzeigen, Auflisten | Berechtigung zum Anzeigen für VPC |
| Adresspräfixe für VPC |  Erstellen, Aktualisieren, Löschen | Berechtigung zum Aktualisieren für VPC |
| Adresspräfixe für VPC |  Anzeigen, Auflisten | Berechtigung zum Anzeigen für VPC  |
|—————|——————|———————|
| Variable IP-Adresse (nicht zugeordnet) | Erstellen, Aktualisieren, Löschen | Alle Kontobenutzer und Berechtigungen zum Anzeigen für alle Kontomanagement-Services, wenn die Ressource in der Standardressourcengruppe erstellt wird. Klicken Sie [hier](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources#setting-up-viewer-access), um Anweisungen zum Einrichten des Zugriffs für Anzeigeberechtigte zu erhalten. **Hinweis: Das Zuordnen und Aufheben von Zuordnungen sind Teil der Aktualisierungsoperation für variable IP-Adressen**|
| Variable IP-Adresse (nicht zugeordnet) | Anzeigen, Auflisten | Kontobenutzer |
| Variable IP-Adresse (zugeordnet) | Aktualisieren | Berechtigung zum Aktualisieren für VPC des zugehörigen Teilnetzes (nachdem sie zugeordnet wurde, kann eine variable IP-Adresse nicht erstellt oder gelöscht werden) |
| Variable IP-Adresse (zugeordnet) | Anzeigen, Auflisten | Berechtigung zum Anzeigen für PC des zugeordneten Teilnetzes der variablen IP-Adressen | 
|——————|———————|————————|
| Netz-ACL (nicht zugeordnet), ACL-Regeln | Erstellen, Aktualisieren, Löschen | Jeder Kontobenutzer |
| Netz-ACL (nicht zugeordnet), ACL-Regeln | Anzeigen, Auflisten | Jeder Kontobenutzer |
| Netz-ACL (zugeordnet), ACL-Regeln | Erstellen, Aktualisieren, Löschen | Berechtigungen zum Aktualisieren für alle zugeordneten Teilnetze, die VPC zugeordnet sind |
| Netz-ACL (zugeordnet), ACL-Regeln | Anzeigen, Auflisten | Berechtigung zum Anzeigen für mindestens eines der zugeordneten Teilnetze, die VPC zugeordnet sind |
|——————|———————|————————|
| Öffentliches Gateway | Erstellen, Aktualisieren, Löschen |  Berechtigung zum Aktualisieren für VPC des PGW |
| Öffentliches Gateway | Anzeigen, Auflisten | Berechtigung zum Anzeigen für VPC des PGW |
|—————————|————————|———————————|
| Geografie | Anzeigen, Auflisten |  Für Regionen und Zonen, jeder Kontobenutzer |
|———————|————————|—————————|
| SSH-Schlüssel | Erstellen, Aktualisieren, Löschen | Berechtigung zum Aktualisieren für den SSH-Schlüssel |
| SSH-Schlüssel | Anzeigen, Auflisten | Berechtigung zum Anzeigen für den SSH-Schlüssel |
|————————|—————————|————————|
| Teilnetz | Erstellen, Aktualisieren, Löschen | Berechtigung zum Aktualisieren für VPC des Teilnetzes |
| Teilnetz | Anzeigen, Auflisten | Berechtigung zum Anzeigen für VPC des Teilnetzes |
| Teilnetz-ACL oder PGW | Zuordnen, Zuordnung aufheben | Berechtigung zum Aktualisieren für VPC des Teilnetzes |
| Teilnetz-ACL oder PGW | Anzeigen, Auflisten | Berechtigung zum Anzeigen für VPC des Teilnetzes |
|——————|—————————|————————|
| Sicherheitsgruppe | Abrufen       | Berechtigung zum Anzeigen für die Sicherheitsgruppe | Sicherheitsgruppe | Auflisten    | Berechtigung zum Anzeigen der Sicherheitsgruppe (andernfalls wird die Gruppe von der Antwort ausgeschlossen)
| Sicherheitsgruppe | Erstellen     | Berechtigung zum Bearbeiten für die Sicherheitsgruppe, die erstellt wird (zum Beispiel eine Berechtigung zum Bearbeiten für alle Sicherheitsgruppen oder eine Berechtigung zum Bearbeiten der Ressourcengruppe, in der die Sicherheitsgruppe erstellt wird.) <br />Berechtigung zum Anzeigen für VPC, in der die Gruppe erstellt wird.
| Sicherheitsgruppe | Aktualisieren/Löschen | Berechtigung zum Bearbeiten für die Sicherheitsgruppe | Sicherheitsgruppenregel | Abrufen/Auflisten | Berechtigung zum Anzeigen für die Sicherheitsgruppe | Sicherheitsgruppenregel | Erstellen/Aktualisieren/Löschen | Berechtigung zum Bearbeiten für die Sicherheitsgruppe | Netzschnittstelle für Sicherheitsgruppe | Abrufen | Berechtigung zum Anzeigen für die Sicherheitsgruppe. <br /> Berechtigung zum Anzeigen für die Instanz. | Netzschnittstelle für Sicherheitsgruppe | Auflisten | Berechtigung zum Anzeigen der Sicherheitsgruppe (andernfalls wird die Antwort 404 zurückgegeben)
<br /> Berechtigung zum Anzeigen für die Instanz, zu der die Netzschnittstelle gehört (andernfalls wird die Schnittstelle von der Antwort ausgeschlossen)
| Netzschnittstelle für Sicherheitsgruppe | Zuordnen/Zuordnung aufheben | Berechtigung zum Betreiben für die Sicherheitsgruppe. <br /> Berechtigung zum Bearbeiten für die Instanz, zu der die Netzschnittstelle gehört.
|—————————|—————————|—————————|
| Images | Anzeigen, Auflisten  | Jeder Kontobenutzer |
|—————————|—————————|—————————|
| Instanzen | Erstellen| Berechtigung zum Aktualisieren für die Instanz und den Datenträger <br />Berechtigung zum Betreiben für die VPC<br />Berechtigung zum Betreiben für die Sicherheitsgruppe, wenn sie angegeben ist |
| Instanzen | Aktualisieren, Löschen | Berechtigung zum Aktualisieren für die Instanz |
| Instanzen | Anzeigen, Auflisten  | Berechtigung zum Anzeigen für die Instanz |
| Instanzaktionen | Erstellen, Aktualisieren, Löschen | Berechtigung zum Aktualisieren für die Instanz |
| Instanzaktionen, Initialisierung, NICs | Anzeigen, Auflisten  | Berechtigung zum Anzeigen für die Instanz |
| Variable IP-Adressen der Instanz | Anzeigen, Auflisten | Berechtigung zum Anzeigen für VPC der Instanz und der zugeordneten Teilnetze |
| Variable IP-Adressen der Instanz | Zuordnen | Berechtigung zum Aktualisieren für die Instanz <br />Berechtigung zum Betreiben für VPC des zugeordneten Teilnetzes |
| Variable IP-Adressen der Instanz | Zuordnung aufheben | Berechtigung zum Aktualisieren für die Instanz |
|————————|——————|————————|
| VPN-Gateway | Erstellen, Aktualisieren, Löschen | Berechtigung zum Aktualisieren für das VPN |
| VPN-Gateway | Anzeigen, Auflisten  | Berechtigung zum Anzeigen für das VPN |
| VPN-Gateway-Verbindungen | Erstellen, Aktualisieren, Löschen | Berechtigung zum Aktualisieren für das VPN |
| VPN-Gateway-Verbindungen | Anzeigen, Auflisten  | Berechtigung zum Anzeigen für das VPN |
| VPN-Gateway IKE-Richtlinien, IPSec-Richtlinien und Verbindungen | Erstellen, Aktualisieren, Löschen | Berechtigung zum Aktualisieren für das VPN |
| VPN-Gateway IKE-Richtlinien, IPSec-Richtlinien und Verbindungen|Anzeigen, Auflisten  | Berechtigung zum Anzeigen für das VPN |
|————————|——————|————————|
| Lastausgleichsfunktion | Erstellen, Aktualisieren, Löschen | Berechtigung zum Aktualisieren der Lastausgleichsfunktion |
| Lastausgleichsfunktion | Anzeigen, Auflisten  | Berechtigung zum Anzeigen für die Lastausgleichsfunkion |
| Lastausgleichsfunktion, Pools und Listener | Erstellen, Aktualisieren, Löschen | Berechtigung zum Aktualisieren der Lastausgleichsfunktion |
| Lastausgleichsfunktion, Pools und Listener | Anzeigen, Auflisten  | Berechtigung zum Anzeigen für die Lastausgleichsfunkion |
|————————|——————|————————|
| Datenträger | Erstellen, Aktualisieren, Löschen | Berechtigung zum Aktualisieren für den Datenträger
| Datenträger | Anzeigen, Auflisten  | Berechtigung zum Anzeigen für den Datenträger |
| Datenträgerprofile | Anzeigen, Auflisten  | Jeder Kontobenutzer |


