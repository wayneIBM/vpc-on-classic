---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, control

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

# Rollenbasierten Zugriff auf VPC-Ressourcen zuordnen
{: #assigning-role-based-access-to-vpc-resources}

Kontoadministratoren können _Berechtigungsrichtlinien_ verwenden, die basierend auf der _Rolle_ eines Benutzers den Zugriff auf _Ressourcen_ steuern. Richtlinien können auch angewendet werden für:

(1) die Autorisierung einer Gruppe von Benutzern, die als _Zugriffsgruppe_ bezeichnet wird,

(2) die zugeordneten Merkmale einer einzelnen _Ressource_ oder

(3) eine Gruppe von Ressourcen, die als _Ressourcengruppe_ bezeichnet wird.

Die Berechtigungen für Ressourcen und die Berechtigungen für Benutzer können unabhängig voneinander zugeordnet werden.

Weitere Informationen zum Erstellen von Benutzern, Benutzerzugriffsgruppen, Ressourcengruppen und Richtlinien finden Sie unter [Informationen zu Ressourcen](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources).

## IAM-basierte Zugriffssteuerung
{: #iam-based-access-control}

Im Allgemeinen sind die Verfahren für Berechtigungen und das Ressourcenmanagement für {{site.data.keyword.cloud}} Virtual Private Cloud auf die IBM Cloud-IAM-Services (IAM, Identity and Access Management) abgestimmt. Weitere Informationen zu IAM, Ressourcengruppen und Zugriffsgruppen im Allgemeinen finden Sie in den folgenden IBM Cloud-Dokumenten:

* [IBM Cloud-IAM](/docs/iam?topic=iam-getstarted)
* [Ressourcengruppen](/docs/overview?topic=overview-whatis-rgs)
* [Zugriffsgruppen](/docs/overview?topic=overview-cloudaccess)

Bestimmte Funktionen des IBM Cloud-IAM-Service wurden für die Verwendung in IBM Cloud VPC angepasst. Im Abschnitt [Informationen zu Ressourcen](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources) werden die IAM-Berechtigungsrichtlinien und ihre Anwendungsweise in VPC erläutert.

Sie können also IAM-basierte Berechtigungen zuordnen basierend auf:

* einzelnen Benutzern
* Zugriffsgruppen (Gruppen von Benutzern)
* bestimmten Typen von Ressourcen
* Ressourcengruppen

## Benutzerberechtigungen zuordnen
{: #assigning-user-permissions}

Für Benutzer wird der Zugriff gesteuert, indem systemdefinierte IAM-Rollen zugeordnet werden:

* Administrator
* Editor
* Operator
* Anzeigeberechtigter

Nachfolgend finden Sie eine Tabelle, die die Analogie zwischen den einzelnen Benutzerrollen und der Art der entsprechenden Kontrolle für VPCs zeigt:

| Rollename | Zulässige Art des Zugriffs |
|-----------|-------------------------|
| Anzeigeberechtigter | VPC anzeigen, VPCs auflisten  |
| Editor | VPC anzeigen, VPCs auflisten, VPCs erstellen, VPCs löschen, VPCs aktualisieren |
| Operator  | VPC anzeigen, VPCs auflisten |
| Administrator |VPC anzeigen, VPCs auflisten, VPCs erstellen, VPCs löschen, VPCs aktualisieren, Richtlinien anderen Benutzern zuordnen |


## Nächste Schritte
{: #permissions-next-steps}

Im Abschnitt [Benutzerberechtigungen für VPC-Ressourcen verwalten](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources) finden Sie schrittweise Anweisungen zum Erteilen rollenbasierter Berechtigungen für Benutzer für bestimmte Tasks.
