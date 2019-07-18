---

copyright:
  years: 2019
lastupdated: "2019-06-13"

keywords: release notes, changes, updates, vpc, profile, hyper protect, estimator, load balancer

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

# Releaseinformationen
{: #release-notes}

Lesen Sie diese Releaseinformationen, um Informationen zu den neuesten Änderungen in {{site.data.keyword.cloud}} Virtual Private Cloud zu erhalten.
{:shortdesc}

## 14. Juni 2019
{: #june-14-2019}

**Aktualisierungen der IBM Cloud VPC-Benutzerschnittstelle**

- **SSH-Schlüssel und Ressourcengruppen.**
    * Ressourcengruppen werden in der Listenansicht der SSH-Schlüssel angezeigt.
    * Ressourcengruppen können im Modaldialog für die Bereitstellung von SSH-Schlüsseln ausgewählt werden.
- **Profile und Bandbreite.** Die Bandbreitenkapazität, die jedem Profil zugeordnet ist, wird auf den Anzeigeseiten **Gängige Profile** und **Alle Profile** angezeigt.
- **Ressourcengruppen.** Auf der Seite für die VSI-Bereitstellung wird das Dropdown-Menü für Ressourcengruppen angezeigt.

**Aktualisierungen für Blockstorage for VPC**
- Die **Länge des Datenträgernamens** wurde auf 63 alphanumerische Zeichen erhöht.
- Die folgenden **Datenträgerfehlercodes** wurden umbenannt und Nachrichten aktualisiert oder gelöscht:
    * `volume_encryption_key_account_id_mismatch` wurde in `volume_crn_account_id_mismatch` umbenannt.
    * `volume_encryption_key_cname_mismatch` wurde in `volume_crn_cname_mismatch` umbenannt.
    * `volume_encryption_key_region_not_found` wurde entfernt.

**Aktualisierungen am SDK**

- **Terraform Provider Version 0.17.1** wurde freigegeben. Laden Sie die [neueste Binärdatei](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1) herunter.
- Mit der neuesten Version von Terraform Provider wurde auch das **Docker-Image** aktualisiert. Das neueste Docker-Image können Sie mit dem folgenden Befehl extrahieren: `docker pull ibmterraform/terraform-provider-ibm-docker:latest`
- Stellen Sie sicher, dass `generation` als Providerargument festgelegt ist, oder exportieren Sie `IC_GENERATION= 1`, sodass der Code mit der derzeit freigegebenen Version der Virtual Private Cloud on Classic-Infrastruktur funktioniert. Als Standardwert ist 2 eingestellt (VPC ist in Vorbereitung, jetzt ist eine Betaversion verfügbar).
- Da keine Providerargumente (`bluemix_api_key` und `bluemix_timeout`) mehr verwendet werden, kann eine Warnung ausgegeben werden, wenn ein Terraform-Plan ausgeführt oder angewendet wird.

## 7. Juni 2019
{: #june-07-2019}

- **IBM Cloud VPC ist jetzt allgemein verfügbar.** Für alle [nutzungsabhängigen Konten](/docs/account?topic=account-accounts) können VPC-Ressourcen bereitgestellt werden. Melden Sie sich an der [IBM Cloud Console](https://{DomainName}/vpc/overview) an und fangen Sie noch heute an.

- **Jetzt können individuelle Berechtigungsrichtlinien für Instanzen, Schlüssel, Datenträger und Sicherheitsgruppen festgelegt werden.** Informationen zu den Rollen, die für die Verwaltung dieser Ressourcen erforderlich sind, finden Sie in [Erforderliche Ressourcenberechtigungen für API- und CLI-Aufrufe](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls). Benutzer des Early Access-Plans sind von den bestehenden Richtlinien nicht betroffen.

- **Der Portgeschwindigkeitsparameter für eine virtuelle Netzschnittstelle wurde aus der Benutzerschnittstelle, der Befehlszeilenschnittstelle und der API entfernt.**

- **In der Benutzerschnittstelle ist jetzt eine Unterstützung für mehrere Regionen für Überwachungsmetriken verfügbar.**


## 31. Mai 2019
{: #may-31-2019}

**Neue API-Version verfügbar.** Die API-Version von Virtual Pirvate Cloud on Classic wurde von `2019-01-01` auf `2019-05-31` aktualisiert. Informationen zu Richtlinien und bewährten Verfahren finden Sie unter [Versionssteuerung](https://{DomainName}/apidocs/vpc-on-classic#versioning) in Virtual Private Cloud on Classic.

## 24. Mai 2019
{: #may-24-2019}

- **Instanzen der Lastausgleichsfunktion, die sich im Löschstatus befinden, werden jetzt in der Listenansicht in der Benutzerschnittstelle angezeigt.** Wenn Sie in der Benutzerschnittstelle das Löschen einer Lastausgleichsfunktion anfordern, wird die Lastausgleichsfunktion weiterhin in der Listenansicht angezeigt, bis der Löschvorgang abgeschlossen ist.

- **Die Funktionen der Lastausgleichsfunktionen mit Layer-7 sind in der Benutzerschnittstelle verfügbar.** Sie können die Funktionen der Lastausgleichsfunktionen mit Layer-7 in der Benutzerschnittstelle konfigurieren.

- **Die Anzeige- und Bearbeitungsrichtlinien der Lastausgleichsfunktion sind jetzt in der Benutzerschnittstelle verfügbar.** In der Benutzerschnittstelle steht jetzt eine neue Funktion zum Anzeigen und Bearbeiten der Richtlinien der Lastausgleichsfunktion zur Verfügung.

Weitere Informationen finden Sie in der [Dokumentation zur Lastausgleichsfunktion](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).


## 10. Mai 2019
{: #may-10-2019}


- **Profilnamen wurden geändert.** Die Instanzprofilnamen haben sich geändert. Befehle zum Bereitstellen von Instanzen müssen aktualisiert werden, damit die neuen Profilnamen verwendet werden können. Weitere Informationen [zu Profilen finden Sie hier](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles).

- **In der Benutzerschnittstelle ist die geschätzte monatliche Preisbestimmung für variable IP-Adressen verfügbar.** Wenn Sie jetzt eine variable IP-Adresse reservieren, wird eine Linie angezeigt, die den geschätzten monatlichen Preis für diese variable IP-Adresse angibt.

- **Unterstützung für Hyper Protect ist verfügbar.**

- **Teilnetzname und Zone werden als Links angezeigt, die mit dem zugeordneten Teilnetz in der Benutzerschnittstelle verknüpft sind**. Teilnetze werden jetzt nach Namen und nach Teilnetz-ID aufgelistet, und der Name fungiert als Link zur Seite mit den Teilnetzdetails für das zugeordnete Teilnetz.

- **In der Benutzerschnittstelle ist eine Schätzfunktion für die Kostenübersicht verfügbar.**

- **Das Abfragen der Pools und Listener der Lastausgleichsfunktion spiegelt sind in der Benutzerschnittstelle wieder.**

    * Auf der Seite mit den Lastausgleichspools wird nun ein Zähler angezeigt, von dem unterhalb der Ressourcentabelle die letzte Aktualisierung angegeben wird.
    * Das Standardabfrageintervall ist 30 Sekunden.
    * Es können vier (4) verschiedene Statusarten angezeigt werden: `newProvision`, `active`, `failed` und `all other states`.
        * Für 'newProvision' gilt: Das Intervall beträgt 15 Sekunden. Die Pools und Listener wechseln sofort in den Status 'active'.
        * Für 'active' gilt: Die Ressourcenliste und der Header werden in Abständen von 60 Minuten aktualisiert.
        * Für 'failed' gilt: Das Abfragen wird gestoppt.
        * Für 'all other states' gilt: Das Abfragen wird in Intervallen von 30 Sekunden fortgesetzt.
    * Wenn Sie einen Listener löschen, kann dies zur Folge haben, dass der Header in den Status `Updating (Actions Unavailable)` wechselt.
    * Beachten Sie auch, dass die Headerinformationen aktualisiert werden, wenn eine Aktualisierung durchgeführt wird.
    * Dieselben Änderungen gelten für das Abfragen für Listener.

- **Die Anzahl der Mitglieder einer Lastausgleichsgruppe wird auf der Mitglieder- und Detailseite in der Benutzerschnittstelle angezeigt, während das Abrufen der Mitglieder durchgeführt wird.**

    * Im Abschnitt für den Allgemeinzustand wird die Mitgliederanzahl angezeigt, während daneben angegeben wird, dass die Instanzdetails abgerufen werden.
    * In der Instanzspalte in der Ressourcentabelle wird die Mitgliederanzahl angezeigt, während daneben angegeben wird, dass die Instanzdetails abgerufen werden.
    * Nach dem Laden wird ein Warnsymbol angezeigt, wenn ungültige Mitglieder vorhanden sind.

- **Auf der VPC-Detailseite in der Benutzerschnittstelle werden alle zugeordneten Teilnetze angezeigt.**
